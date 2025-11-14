---
title: Building a Complete Android App + Backend With Claude
date: "2025-11-14"
description: "I ran an experiment to build a full Android app, along with a functional backend, using Claude. The goal was to figure out exactly where I stood on LLM's and their usefulness. This post details my thoughts after the development process."
tags: ["LLM","ai", "opinion", "development", "learning"]
---

There's been a lot of discourse lately about LLMs and how useful they are. I've said [before](https://jcachada.dev/weblog/you-dont-own-my-brain-the-problem-with-ai-fud/) that I'm not a huge fan of LLMs and I have a lot of conceptual problems, that range anywhere from "it's horrible for the climate and a violation of copyright", to "it's another step in the Big Tech oligarchy taking over every aspect of our lives and making them measurably worse". Those things worry me a lot still, and I don't have a clever solution to them. 

But one of the criticisms that I've engaged with a lot is the idea that they are useless. There's a lot of discourse going around that is some variation of "LLM's are not doing the things I want them to do, so I don't see the usefulness." At the same time, they are being pushed very hard by Big Tech, by companies all around the globe, and by people in positions of power in businesses. While it would be very tempting to think these are all shills with no social conscience who are simply caving to the tech bro oligarchy, that feels like the lazy way out. There are a lot of smart people involved in these pushes, and I'd be surprised if they were all just losing the plot. 

As a software tradesman in my current career, I also feel like I have a responsibility to engage with these tools in some form. I can't just afford to be unequipped to engage in the discussion on an actual technical level - even if I end up being a detractor in internal conversations, I need to actually have done the legwork to back up those positions. I wish appeals to morals were enough, but reality often works differently. 

So I set out to engage with the LLMs in a constrained experiment to try and answer that specific question - do they work?  

## The methodology

The way I went about this experimentation is quite simple: I tried to build a full app, from scratch, comprised of a backend and an android app, by **only** using Claude. I chose Claude because it was, at the time, the LLM people in my circles seemed to agree was best for coding tasks. And I chose this task because it is something I could do on my own, but only by putting a significant amount of time into it. This way, I hoped I could figure out if the LLM was actually speeding me up, or if it was making me slower in the end. 

I decided to try this in two ways: 

1. The first way was to just open up a Claude Code terminal and start coding. I explained my ideas as I went and winged it as best I could. I nudged it when it lost track of things, provided as much guidance as I could, but engaged with it only through the terminal (and through the instructions in my `CLAUDE.MD`).

2. The second way was a little more involved. I researched different ways people were using these LLMs, and used a variation of [this](https://harper.blog/2025/05/08/basic-claude-code/) method. To summarize it, I used a reasoning model (Chat GPT in thinking mode) to discuss the plan first and build up a spec for my project. You can see the outputs in [this] folder. Then, I asked the same reasoning model to build me a prompt plan, telling it explicitly that I would feed these prompts to Claude. I used these as a base for the project.

## The rules

Throughout both attempts, a few rules held true:

1. I did not write a single line of code manually. Everything in the project was input by Claude; I wanted to limit test how far it could go, and interfering would have made it harder to tell.

2. I did not try different LLMs for the same task.

3. I generally used the same context window for every day (a single running terminal instance). On certain occasions, when Claude was really hallucinating, I started a new terminal from scratch. Every time I did this, I asked it to restart from the context `.md` files I linked above, that keep track of the current state of the project across sessions.

4. I never used `auto-accept`. I read every suggestion Claude gave me and gave it nudges throughout the experiment. The frequency of the nudges varied with the quality of the suggestions. 

## The results

The code for this experiment can be found [here](https://github.com/JCachada/dare-app-public).

<p align="center">
<img src="/images/dare_app_screenshot.png" alt="A screenshot of an android screen showing an application for repeatable tasks, in black and dark red themes, with buttons for Prizes, Dares and Settings." style="width:200px;"/>
</p>

The app is fully functional, and the github repo would allow you to deploy it (although you'd have to deploy both the backend and install the APKs on your phone). The main limitation is that it does not support full authentication, but rather a very naive sort of thing, where the API endpoints are publicly exposed (not even using Bearer Tokens / JWTs), so as long as you know your session id and the URL of the deployment, you can write prizes or dares. This is insecure, but it was good enough for the experiments I wanted to run.

## The learnings

So, what did this all actually teach me? A few things.

First of all, **LLMs are not useless** from a purely pragmatic point of view. They can and will speed you up at certain things, especially if you know how to direct them. For example, I am confident it would have taken me over a day to set up a proper Android development environment where I could run my tests. I would also have taken at least a couple of days throughout the project to actually write those tests. Using Claude significantly accelerated both of these processes. They did equally well at writing code with very strict parameters and specifications, and at doing very repetitive tasks like writing unit tests or making sure they all conformed to the same Arrange/Act/Assert format.

However, their usefulness is **heavily dependent on the mode of usage**. The first way I tried using LLMs with (the one where I just talked to it in a terminal session) led absolutely nowhere. I ended up quitting in frustration after a week, after a particularly frustrating day of constant hallucinations. The second method was night and day - with a clear spec and a much more defined scope, things went a lot smoother and I actually got to a functioning end result. Claude is good at following clear, unambiguous instructions. It seems very important for its usefulness that by the time it's called upon, you already know exactly what it is meant to help you with. In my experiments, it did very poorly at trying to problem solve *and* write code at the same time. This makes me think that at least a part of the crowd that has found LLMs useless (and is not just externalizing a bias against those LLMs, however valid the reasons to do so may be) is struggling with the ways to use it, more than the tool's capabilities itself. A hammer is not a great tool if you talk to it about your podcast idea.

Another important note is that LLMs are **not reliable**. After running this experiment, I would never give LLMs elevated permissions, like database access or even the ability to commit. Even though my `CLAUDE.md` file explicitly states to never commit things without passing tests, and I reiterated it multiple times throughout the sessions, I still found that Claude often ignored me and tried to hack its way past a bad state. I've catalogued all instances where Claude commited failed builds [here](https://github.com/JCachada/dare-app-public/blob/main/llm_files/build_history.md). As you can see, it commited broken builds over 25 times. 3 of those are somewhat excusable: the ones listed as API/Contract issues went away once I asked Claude to introduce contract tests. However, the others are inexcusable. Claude committed broken tests, builds that wouldn't compile, and sometimes even tried to just comment out the test to be able to move on, despite frequent and explicit guidance not to do so. 

This is also why I would **never trust an LLM in auto-accept mode**. I let Claude commit these so the failures would be reflected in the `git` history, but most of them I saw happening in real time when I was reviewing the code it proposed. If I had not reviewed all of the code as it was written and provided prompt corrections, these would have been far more than 25 broken builds. Which leads me to... 

## The conclusion

One of the things this has cemented for me is that LLMs can function as an accelerator, but they can not function as a way to plug holes. What I mean by this is: **you can sometimes use LLMs to accelerate things that you know how to do, but would take you a long time**. If you provide close supervision and clear scope, they can save a lot of menial work. And they can be useful for compounding on knowledge you already have, provided you have enough knowledge of the subject matter to call its bullshit. 

However, they **cannot be used to do things you don't know how to do in the first place**. The main reason for this is that you won't be able to tell when it's messing up; it often does so in perfidious ways that kind of sound correct, but will bite you (hard) in the long term if you let them slip through. In this case, I would recommend trying to learn how to do the thing first. 

This means, with a gross oversimplification: if you're a software engineer, you can probably get some value out of an LLM for writing code. If you're a product manager, please ask your engineers to code for you instead. I imagine this works similarly for other roles: if you're a translator or a copywriter, I imagine you can spitball some options with an LLM, but you have to have enough subject matter expertise to then keep it on track, and to catch it when it inevitably fucks up. 

## The gambling hot take

One funny thing that I realized while using the LLM is that it causes a different emotional response in me than normal code writing. Writing code is a grind; you systematically work away at a problem and rule out options for solving it, refine your approach, iterate until you have something that you're happy with. It's an almost meditative process of approaching something workable through methodical problem solving, at least for me.

When you introduce an LLM, the incentives change, and it becomes more like gambling. I felt the same parts of my brain activate as when I play a quick card game or a round of Balatro. With LLMs, I'm prompting the slot machine and banking on it putting out the right answer; when it did, it felt less like the delayed, long lasting gratification of wrapping up a complex problem through my subject matter expertise, and more like getting the exact right draw at the exact right time, or getting a particularly satisfying critical hit in a fighting game. It was dopamine fast food instead of what I usually get out of coding. I thought that was an interesting thing to see in action, and it might explain why developers sometimes trend towards using tools like LLMs even when they are not the right fit for the job, or when they are not actually giving them any productivity gains. 

## Disclaimer

Like I've mentioned many times, I think part of deciding where I stand on LLMs and the current state of the tech industry is a complex process that **requires** engaging with the tools in good faith before I take an informed position. This experiment was an exercise in trying to do just that. I'm not endorsing LLM usage - in fact, if anything, I still very much trend in the direction of *we're burning up our planet for a chatbot to line the wallets of despicable people*. Recognizing that LLMs have legitimate usages does not necessarily threaten that; if anything, saying that I feel that way *even if they have legitimate usefulness* is proof of how strongly I feel about those downsides. 

What I'm saying is - please don't yell at me or send me e-mails complaining about how I'm furthering the Big Tech world takeover by using Claude for this. I believe having mature positions on LLM discourse requires some level of actual engagement, and that no one should be discouraged from trying to get to a more informed position. 

If you have legitimate issues or arguments in good faith you want to raise, as usual, I am all ears.