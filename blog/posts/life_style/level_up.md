---
title: Level UP my workflow as a software engineer and ML researcher 
description: A rough thoughts about my workflow as a software engineer and ML researcher, the problems I face and how I will try to solve them.
date: 2025-05-27
categories: [blogging, idea_Forge, publish, rough_thoughts, life_style]
order: 1
draft: true
featured: true
author: kareem
---

## Level UP

I don't know what title should i pick for this article.. that will not get finished..soon.

I am a human trying to be a good software engineer and ML researcher in the same time while doing other stuff as a human not a bot.

The last 6 months, i was doing great thinkg, learning a lot of new cool AI stuff and started working on multiple projects (opensource, Master thesis, research paper..etc)

I finished only one or two from around 20 projects!!!

This is big problem, knowing that most of time i am looked at my room with kobo (my little dragon who can't breathe flames!🔥) .....

time pass and i am still in my room doing everything without finishing anyting, only my mental health, life(time).

> fun fact => I don't have Instagram, ticktock, google chrome and don't play games.

I will try to describe what is my problems and suggestion to fix it..this will change weekly or more until i am satisfied with what i wrote and achieved.

### What is the Goal? solving problems

My goal is to finish what i started!
Examples of the projects i am working on :

1. Train a Vision Encoder , OCR and Embedding models
2. shipping some full-stack project into production with AI features
3. Finish my Master Thesis, wrote/read papers about certain topics.

what prevent me from that...i am trying to figure but let's discuss it here from my point of view.

I will divide them into the following category so it will be easier to tackle them

### Coding Env

I can't forget the vibe related to doing problem solving...it's a totally different experience... what i found similar is web development.
In problem solving all your focus is one thing solve this problem.. you have a coding setup which pull the problems from code-forces and your snippets are ready..the only missing thing is to think and try, get error and try...this a focused session it can take from 20 minutes to more more than 12 hours doing problem solving without any get bored..you are in the Flow experience.

what i found similar experience is creating fronted projects..the quick feedback of creating a thing and seeing it on your screen with incremental updates give me the flow because i can do what i want in this screen from the database.. you are in the Flow if you doing this with HTMX :)

When i come to do ML i will face the following:

#### Broken code bases

It's everywhere, try to search for any research paper implementation, AI opensource project and everything will be broken, not functional, out of date.

There is no standard for anything..what works on A100 GPUs will not work on RTX 2060.. frequent updates, complicated AI frameworks (llama-index and lang-chain) it's easy to start with them...then you are you locked.

Found an interesting paper or model and want to read the code and adopt it for your case...guess what you are late, it's has been released 6 months and will not work :)

please don't use faiss..please!

#### Bad programming skills

Most of use are doing AI in notebooks and this is very cool and i will not stop use it..but write software for other people to use and read that will work in production.
everything need to change.

you can use nbdev and it will change your coding experience..but people outside of fastai courses found it hard to understand, bad to add and they don't the code to be written in this way. it's not best practices from their side.

The problems increases when you release you don't know how to :

1. write/understand multiprocessing code
2. can't create/update these advanced python classes.
3. multi-gpu problems
   more to add later.

#### GPU poor

You know that you can't train LLMs from scratch... so let's go to fine-tune vlm, representation models.

it's good spaces to work with and i really like IR, searching and vlm world.
but you can't do a lot on your laptop gpu 1660TI.

you can use cloud compute... i am not that rich and paying in dollar is very expensive.

what i want to say that a lot of tasks you need to do depends on having enough compute power.
people in Europe are sad because they can't only try experiments with their dual 3090 or fine-tune embedding models on 4080 TI.
There is also people how don't have the minimum so the learning experience is insane.

Kaggle helped a lot but 30/week hours are not enough.
but the problem is not with the enough compute but with the waiting to the task to finish.

imagine if you were a web developer and to deploy your project in dev mode....need to wait days until it finish!

sometimes i have access to a Titan RTX GPU 24 GB. and try to fine-tune a model or create a dataset ... i have to run it for 2 days.

within this time... i need to continue work !
so i will try to find a gpu and wait for another days.
within a week i was working on 3 different project and didn't complete any of them yet!

#### AI steal your flow

I think using AI tools is very helpful but using it wise...or you will lose more than you think.
you really didn't be productive when you vibe code a project in less than 1 hour and stay week trying to create a small change.!!
if you spend 2 days with help from AI as a search tool not writing..you will be amazed by how much you can do and the experience.

what i am using as my AI tool?

I use all of them for searching the web and for the coding help, i fill in love with solveit from answerdotai.

#### Math

a lot of strange math and this enough

### Real Env

- Internet and electricity connections
- when i use dual screen setup it make the expirence better but it encourage me to surf the web/social media.
- I feel comfortable with one-screen but there is no enough screen for these open windows with these lengthy files
- noise..much of noise

### Negative thoughts

- you can't do this
- you are not good enough.
- X didn't love me anymore
- i will not have time to do X

---

- Personal/family issues

### Low Power 🔋

- bad eating habbits
- low quality sleep
- a lot of anxiety..you didn't finish you work!
- long hours of sitting
  **what about motivation?**
  i don't care about..and you shouldn't also

## Fooling yourself

1. new ideas(projects/research)
2. i want to stay updated with new models, libraries..etc
3. you are don't doing what really matters
4. 80-20%
5. learning Rust!

## Solutions

i have some of ideas i will try to apply to my life and if it worked i will share it but the i want to follow these rules :

1. don't let your project open-ended ..set time for every task
2. don't waste your will power
3. this applies for even time-tracking, habbit tracking..etc.
4. don't waste your attention power
   what i am trying now?

- I started to use better window management i3 in my popos machine: it helped me to get rid of the habbit of second montior....one is better and make me focused more also i think after some time with i3 my muscle memory start to apply and the distraction from manage multiple windows and with mouse and other bad spaces tabs.
  **number spaces are better than just one-above and one-below system in popos**
- I am using super productivity app to manage my tasks..i need to optimize the schedule tasks
- I created 4 cli commands to use within my terminal while waiting for python interpreter to finish

1.  **inner thoughts logging** => open the daily note with helix editor to add the negative thoughts and ideas in quick way.
2.  **small achievements** => open a note in obsidian for this month about tasks i have done..even small one like i created a blog post about x, solved problem with huggingface transformers...this works as a metric to your progress..and help solve the problem i didn't achieved anything!
3.  **activity_watch** commands => building some activity watch command to tell me what i was doing in the last 2 hours in a nice visualized way .. i will share the code later after some updates and usage to make sure it's functional.... when you see the real picture you will start realize how much you could have done and what you are really doning.. don't fool yourself while you opened substack,x 10 times in hours !!!
4.  blocked all social media and start to use RSS to stay updated with what really matters not just some hybe.
5.  reading two books
6.  deep work
7.  slow productivity
8.  sync my media and reading:
9.  trying to find a way to sync my zotero reading with my tablet
10. trying to create one application to manage my listening media from telegram channels, soundcloud, local files with to be able to listent from my mobile and pc with continues.
11. store all my bookmarks from all social media and platforms into one place and add some ai for it.
12. trying to create an app to take notes from my voice in linux in quick way and optimized for my language.
