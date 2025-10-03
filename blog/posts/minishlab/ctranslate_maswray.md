---
title: From $32,000 to $0 with Small Models and CTranslate2
description: How I reduced translation costs from $32,000 to $0 using small models and CTranslate2 optimization, achieving 4-6x speed improvements
author: kareem
date: 2025-10-03
draft: false
featured: true
categories:
    - machine-learning
    - optimization
    - translation
    - ctranslate2
    - cost-optimization
image: "images/bojji.png"
---

## Going from $32,000 to 0 cost with small models

![faster masrawy](images/bojji.png)

It's friday and i have sometime to continue working on opensource tasks, i had
an idea that requires a translate a dataset it's not large 2GB it consist of
some paragraph from English the average of words is around 400 token each row
from the 8 million :) I get into the OpenAI models to calculate how much this
will cost me to translate all these with gpt4.1 and gpt4.1-mini and the prices
are the following : my total tokens =>8,000,000 rows * 400 toknes =
3,200,000,000 which costs the following as input and output i will assume they
are the same here for the ease of calculations i will use the normal APi not
Batched API:

- total input tokens => 3,200,000,000
- total output tokens => 3,200,000,000

**The calculation based on this date 2025-10-03**

| Cost           | GPT4.1    | GPT4.1-mini  | GPT3.5 Turbo |
| -------------- | --------- | ------------ | ------------ |
| Input tokens   | 3,200 * 2 | 3,200 * 0.40 | 3,200 * 0.50 |
| output outputs | 3,200 * 8 | 3,200 * 1.60 | 3,200 * 1.50 |

| Cost                                                                          | GPT4.1    | GPT4.1-mini | GPT3.5 Turbo |
| ----------------------------------------------------------------------------- | --------- | ----------- | ------------ |
| Input tokens                                                                  | 6400      | 1280        | 1600         |
| output outputs                                                                | 25600     | 5120        | 4800         |
| Total $ cost                                                                  | 32000     | 6400        | 6400         |
| Total EGP cost                                                                | 1,527,680 | 305,536     | 305,536      |
| it's interesting that the cost of gpt4.1 mini is the same as gpt3.5 Turbo ^ ^ |           |             |              |

also this is insane i can afford only 1000 EGP or 50$ at max :)

## How poor I am ? very GPU poor! 💻

I wish has a Local GPU to be able to test Opensource LLMs like Cohere 70B and
such strong models and i will not care about time!

I remeber there is an amazing work that is not even llm for translation created
by [Ahmed Wasfy](https://www.linkedin.com/in/ahmedwasfy/) from
[NAMMA Community](https://www.linkedin.com/company/namaa-community/posts/?feedView=all)

it's a 240M Params small model traied to translate English into Egyptian It was
trained on more than _**150,000**_ rows with more than _**10 Million tokens**_
for Arabic Language it's competes with Closed LLMs like Gpt-4o and
Claude-3-5-sonnet ![[Pasted image 20251003174247.png]]i tested it with my local
laptop gpu 1660Ti GTX mobile version with 6GB and CPU is core i 7 gen9 from
intel with 32GB ddr4 i tested the model and it worked with pytorch very fast and
very efficient! the translation are very similar and this is enough for the task
i want to build on these translated dataset! let's do the math for how much this
will cost on my laptop :)

---

## Pytorch Pipeline with HF Inference through Transformers

i created the translation pipeline and tested it with these batch size with
these settings float16 and batch sizes = [ 2,4,6,8,16] I found that even i have
enough memory to load more batch the optimal batch size was 4 and this is
becuase the ram and CPU power. i was able to process 100 examples in these
results:

| Method                                |   | Number of Examples | Batch Size | GPU setup         | Full time | Days need  |
| ------------------------------------- | - | ------------------ | ---------- | ----------------- | --------- | ---------- |
| Pytorch + float16                     |   | 100                | 8          | 1660TI laptop GPU | 60s       | 55.56 Days |
| Pytorch + float16                     |   | 100                | 4          | 1660TI laptop GPU | 45s       | 41.67 Days |
| Pytorch + float16 + optimized version |   | 100                | 4          | 1660TI laptop GPU | 35s       | 32.41 Days |

optimized version here i mean

```python
model = torch.compile(model, mode="max-autotune", fullgraph=True)

model = model.eval()
```

it's a new feature in [pytorch 2](https://pytorch.org/get-started/pytorch-2-x/)
to accelerate the models speeds i tried multiple settings and also the different
backends but the there is no huge difference convert to onnex? it's a nightmare
i did it before and the results didn't worth the headache! I may be try it when
i have time

## Let's Do Quantization

I Tried to use the [torchao](https://docs.pytorch.org/ao/stable/serving.html)
which enable performing quantization in more stable and easer ways i tried it
alot with my GPU but due to cuda version it always throughs this error:

```bash
1. AssertionError: Float8 dynamic activation quantization is only supported on CUDA>=8.9 and MI300+
```

the library code is not straightforward and i don't have time i have just 2 days
to finish this before i return to my main work.

I also faced some error because the model i am trying to use is an old and not
optimzied one for these method it's based on the
[OPUS-MT-en-ar](https://huggingface.co/Helsinki-NLP/opus-mt-tc-big-en-ar)
created with [marianNMT](https://marian-nmt.github.io/) which is an efficient
NMT implementation written in pure C++. The models have been converted to
pyTorch using the transformers library by huggingface

## VLLMs and SGLang for HF translation pipelie

i search for SGLang solution with the the model and i didn't find any help i
tried the VLLMs documentation also and found the following page
[bring_your_own_model](https://docs.vllm.ai/en/latest/contributing/model/basic.html#1-bring-your-model-code)
and
[this](https://docs.vllm.ai/en/latest/models/supported_models.html#modelscope)
the model speed was worth than normal HF tensors it was more 5 seconds. I think
there is a better way to write the VLLm version better mine.

## More search and Ctranslate magic 🎩

I want to give up , but let's try a final search how to serve marian model and
in an old forum answer i found what is called
[Ctranslate](https://opennmt.net/CTranslate2/quickstart.html) they say it's
faster than HF transformer for specific architecture by around 4-6x

#defintion CTranslate2 is a C++ and Python library for efficient inference with
Transformer models. The following model types are currently supported:

- Encoder-decoder models: Transformer base/big, M2M-100, NLLB, BART, mBART,
  Pegasus, T5, Whisper
- Decoder-only models: GPT-2, GPT-J, GPT-NeoX, OPT, BLOOM, MPT, Llama, Mistral,
  Gemma, CodeGen, GPTBigCode, Falcon, Qwen2
- Encoder-only models: BERT, DistilBERT, XLM-RoBERTa

Compatible models should be first converted into an optimized model format. The
library includes converters for multiple frameworks:

- [OpenNMT-py](https://opennmt.net/CTranslate2/guides/opennmt_py.html)
- [OpenNMT-tf](https://opennmt.net/CTranslate2/guides/opennmt_tf.html)
- [Fairseq](https://opennmt.net/CTranslate2/guides/fairseq.html)
- [Marian](https://opennmt.net/CTranslate2/guides/marian.html)
- [OPUS-MT](https://opennmt.net/CTranslate2/guides/opus_mt.html)
- [Transformers](https://opennmt.net/CTranslate2/guides/transformers.html)

### Key features of Ctranslate

**Fast and efficient execution on CPU and GPU**\
The
execution [is significantly faster and requires less resources](https://github.com/OpenNMT/CTranslate2#benchmarks) than
general-purpose deep learning frameworks on supported models and tasks thanks to
many advanced optimizations: layer fusion, padding removal, batch reordering,
in-place operations, caching mechanism, etc.

- **Quantization and reduced precision**\
  The model serialization and computation support weights
  with [reduced precision](https://opennmt.net/CTranslate2/quantization.html):
  16-bit floating points (FP16), 16-bit brain floating points (BF16), 16-bit
  integers (INT16), 8-bit integers (INT8) and AWQ quantization (INT4).
- **Multiple CPU architectures support**\
  The project supports x86-64 and AArch64/ARM64 processors and integrates
  multiple backends that are optimized for these
  platforms: [Intel MKL](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/onemkl.html), [oneDNN](https://github.com/oneapi-src/oneDNN), [OpenBLAS](https://www.openblas.net/), [Ruy](https://github.com/google/ruy),
  and [Apple Accelerate](https://developer.apple.com/documentation/accelerate).
- **Automatic CPU detection and code dispatch**\
  One binary can include multiple backends (e.g. Intel MKL and oneDNN) and
  instruction set architectures (e.g. AVX, AVX2) that are automatically selected
  at runtime based on the CPU information.
- **Parallel and asynchronous execution**\
  Multiple batches can be processed in parallel and asynchronously using
  multiple GPUs or CPU cores.
- **Dynamic memory usage**\
  The memory usage changes dynamically depending on the request size while still
  meeting performance requirements thanks to caching allocators on both CPU and
  GPU.
- **Lightweight on disk**\
  Quantization can make the models 4 times smaller on disk with minimal accuracy
  loss.
- **Simple integration**\
  The project has few dependencies and exposes simple APIs
  in [Python](https://opennmt.net/CTranslate2/python/overview.html) and C++ to
  cover most integration needs.
- **Configurable and interactive decoding**\
  [Advanced decoding features](https://opennmt.net/CTranslate2/decoding.html) allow
  autocompleting a partial sequence and returning alternatives at a specific
  location in the sequence.
- **Support tensor parallelism for distributed inference**\
  Very large model can be split into multiple GPUs. Following
  this [documentation](https://github.com/OpenNMT/CTranslate2/blob/master/docs/parallel.md#model-and-tensor-parallelism) to
  set up the required environment.

Some of these features are difficult to achieve with standard deep learning
frameworks and are the motivation for this project.

### Let's try it !

i used the following script to convert the HF version into Ctranslate expected
one

```bash
ct2-transformers-converter --model NAMAA-Space/masrawy-english-to-egyptian-arabic-translator-v2.9 --output_dir ct2_model_masrawy
```

then i used the model with this version

```python
translator = ctranslate2.Translator(
	"ct2_model_masrawy",
	device="cuda",
	compute_type="float16",
)
```

it worked and was very fast much, much faster

---

| Method               |   | Number of Examples | Batch Size | GPU setup         | Full time | Days need  |
| -------------------- | - | ------------------ | ---------- | ----------------- | --------- | ---------- |
| Ctranslate + float16 |   | 100                | 8          | 1660TI laptop GPU | 11s       | 10.19 days |
| Ctranslate + float16 |   | 100                | 4          | 1660TI laptop GPU | 16s       | 12.04 days |
| Ctranslate           |   | 100                | 6          | 1660TI laptop GPU | 13s       | 14.81 days |
|                      |   |                    |            |                   |           |            |

we moved from 32.4 Days into 10 Days!!!

## getting help from The Titan RTX 24GB

One of my friends offered me an access to it's Workstation which is dual gpu
Titan RTX it's an old gpu but it's far better than my little
[kobo](https://kareemai.com/blog/posts/mteb_encoding/my_little_dargon.html)

I found that the optimal batch is 64 with after some tries. let's use this and
see the results i will also increase the size from 100 samples into 1000

| Method                                                             |   | Number of Examples | Batch Size | GPU setup      | Full time | Days need  |
| ------------------------------------------------------------------ | - | ------------------ | ---------- | -------------- | --------- | ---------- |
| Pytorch + float16 + optimized version                              |   | 1000               | 64         | Titan RTX      | 240s      | 22.22 days |
| Ctranslate + int8 <br>                                             |   | 1000               | 64         | Titan RTX      | 7.89s     | 0.73 days  |
| Ctranslate + int8 + dual GPU                                       |   | 1000               | 64         | Dual Titan RTX | 4.42s     | 0.41 days  |
| Ctranslate + float16                                               |   | 1000               | 64         | Titan RTX      | 6.46s     | 0.60 days  |
| Ctranslate + float16 + dual GPU                                    |   | 1000               | 64         | Dual Titan RTX | 3.72      | 0.34 days  |
| Ctranslate + int8_float16                                          |   | 1000               | 64         | Titan RTX      | 6.81s     | 0.63 days  |
| Ctranslate + int8_float16 + dual GPU                               |   | 1000               | 64         | Dual Titan RTX | 3.85s     | 0.36 days  |
| ==we moved now from 22 days in single titan rtx into 0.60 days! == |   |                    |            |                |           |            |

#### Why float16 is faster than int8

**Float16 Version (faster!):**

- Dual GPU: **269.2 docs/sec**
- Single GPU: 154.7 docs/sec
- Time for 1000 docs: 3.72 seconds

**Int8 Version (slower):**

- Dual GPU: **225.8 docs/sec**
- Single GPU: 126.7 docs/sec
- Time for 1000 docs: 4.43 seconds

**Result: Float16 is ~19% faster! (269.2 vs 225.8 docs/sec)** **Lower precision
≠ Always faster!** I need to increase the batch size for the int8 and see which
batch size will be now better!

## Next steps!

This is just the start i will search more and invistage how to make this more
faster because the 8 million are only 2GB and the next task is to translate
500GB :) every second will make a huge difference!

- use large batch size with int8
- use different GPU with modern architecture and better CPU
- deep dive into VLLm
- Try the onnex version for GPU not cpu
- try again with torchao

Thanks for your time! here is the converted version on huggingface
[ctranslate_masrawy](https://huggingface.co/Abdelkareem/faster_masrawy) small
models can save your life ^ ^
