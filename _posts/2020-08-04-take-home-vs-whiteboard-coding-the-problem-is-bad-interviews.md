---
layout: post
title:  "Take-home vs whiteboard coding: The problem is bad interviews"
date:   2020-08-04
categories: interviewing software careers
---
**Note:** The article made it to [the front page of hacker news][1]! Lots of great discussion there.

----------

The software engineering field has controversy over using whiteboard coding in interviews, versus candidates doing take-home coding assignments. This is a false dichotomy, the problem is bad interviews.

Quincy Larson, in his post [Why is hiring broken? It starts at the whiteboard][2], gives some examples of bad whiteboard questions. In his article, he slams the practice as either a form of hazing, or he gives examples of horrible questions that rely on the candidate memorizing arcane algorithms.

Yet, the bad examples that Larson lists are the opposite of what Joel Spolsky recommends in [Smart and Gets Things Done][3]. The only thing I can conclude is that Larson is really complaining about bad interviews, not bad technique.

**In this post I focus on the tradeoffs between take-home coding questions, and live coding exercises, such as whiteboard coding.**

## Examples of Bad Take-Home Questions ##

After I wrote the first draft of this post, I actually walked away from a job opportunity that asked me to do a horrible take home exercise. Here's the assignment, summarized to respect the company's privacy:

> This should take a few hours. Create a web-based message board. Use C# and ASP.Net Core, with Angular and Material design. Use a database. When someone adds a message, update all other open browsers in real time. To make it easy for you, you don't need to implement authentication.

This isn't the first time I've walked away from such a situation. **Why did I walk away?** In both cases, it was the same exact reason.

**The exercise was supposed to take "a few hours"** according to the hiring manager, but I'm extremely rusty with ASP. I haven't done anything real time since before WebSocket, nor have I used Angular or Material design. **I anticipate that such a project would take one of their engineers more than "a few hours" to finish, and take me at least a day to come up to speed with all the various APIs and patterns.**

**At this point, we've only had one phone screen.** The company hasn't invested much time in me, but they expected me to invest a lot of time in them. I hadn't met other members of the team, and I've never seen the product.

**I wondered, what if I put a lot of time into this and** make a novice mistake? Will some junior engineer reject me? Is this a sign that someone in a leadership role has unreasonable expectations? What if, at the end of this process, **I just don't want the job?**

**Take-home assignments should never have learning curves.** There are reasons when it's okay to request an alphabet soup of technologies, which I explain a little later.

Another situation where I've walked away was the "fix a bug in our open source project" task. Onboarding on a complicated piece of source code can take a few days. It can be longer if the source code uses a language or idiom that I'm unfamiliar with.

**"Fix a bug in our open source project" always feels like hazing to me**, because I just can't compete fairly with someone who's already worked in that codebase. I always worry that I'll put a lot of time into the take-home and not get an interview.

In another case, a hiring manager emailed me a candidates' take-home coding assignment and told me that I needed to interview the candidate, in-person, later that day. **The take home question was fine, but the candidate's code was awful! We should have rejected the candidate instead of flying the candidate to the office**. Even worse, I suspected that one of my colleagues was rude to the candidate when reviewing the code.

## An Example of a Bad Whiteboarding Question ##

I once interviewed at a company where a young engineer gave me a simple question, which I quickly answered with a straightforward solution. **The engineer then spent the rest of the time berating me for not including an insignificant micro-optimization.**

The interviewer was just as rude as my colleague was when he reviewed the bad take-home assignment. I remember telling the recruiter that I didn't think I'd like the company.

In another case, I interviewed at a video streaming company where the whiteboard question required that I know a textbook algorithm that I would never use directly. **In reality, the question was asked poorly. If they said, "here's the problem and here's the algorithm you need to solve it," it would have been a great question.**

But, more importantly, a bad interview can happen with either a take-home assignment or a whiteboard assignment.

## The Problem is Bad Interviews ##

**It's always important to remember that an interview is a two-way street. It's just as important to sell the job to the candidate as the candidate must sell themselves to you. Normally, it's extremely hard to find software engineers qualified to show up for an interview, so running bad interviews means that the best candidates will just walk away.**

Larson, in his blog post, explains a more disturbing trend:

> Whiteboard interviews are akin to a hazing ritual — a rite of passage that one must endure before joining an organization — because everyone else there did. “I went through a battery of whiteboard interviews when I got here, so why shouldn’t this person?”

I suspect companies who haze potential employees have bigger problems, the first of which is that they aren't coaching their interviewers in how to perform a good interview!

No one ever coached me in how to run a good interview. Instead, I had to take the initiative to read "Smart and Gets Things Done." When I've worked for large tech companies, the HR departments knew less about how to interview a software engineer than we, the developers running the interviews, did.

## What About Interview Websites? How About The Candidate Brings Their Laptop ##

Sites like [https://leetcode.com/][4] and [https://coderpad.io/][5] offer ways to facilitate live and take-home coding questions.

I think these sites are great, especially when they provide a wealth of examples of questions to base an interview on. But, just using a tool won't magically make someone a better interviewer.

**I think the biggest value of these tools is that they respect the candidate's time. Before asking a candidate to spend time traveling to your office, you can do the first part of the interview without the candidate investing travel time.** This is critical if the candidate needs to fly for the interview, but also respectful for candidates on the other side of town.

I also think a great technique is to ask the candidate to bring a laptop to an on-site interview and then watch the candidate code on-screen. As a candidate, I once plugged my laptop into a projector and fired up my IDE. I personally found it easier than scribbling on a whiteboard.

## Tradeoffs Between Whiteboarding vs Take-Home questions ##

When a company relies on take-home questions, it's low-risk for them to send anyone their take-home question, in contrast, an in-person whiteboard question requires a more time from the company's engineers.

But, a company can send out its take-home interview question indiscriminately and waste candidates' time. (This is why I walk away from time-consuming take-homes, or take-homes that require an alphabet soup of technologies.)

It's possible to cheat on a take-home, because a candidate can send it to a stronger programmer, or sit with another programmer. Cheating on a live coding interview is harder.

But, many engineers prefer to code without an audience and feel more comfortable coding in private. The take-home questions often give bigger insight into the candidate's style and ability to organize a program.

Take-home questions give little insight into how a candidate behaves in the office, or has a technical discussion. They also give little insight into a candidate's intuition, because you only see the finished product, not their working process.

With a whiteboard question, or other live coding techniques, it's easy to see a candidate's biases. These biases can expose someone who's going to struggle in your stack, or just someone who you won't get along with. They also can expose when a candidate has poor "common sense" for the kind of work you're doing.

Another advantage of whiteboard questions is that it's easy to tune them on the fly. You can see where a candidate stumbles, and you can infer a bad question based on a candidate's reactions.

It's much harder to tell when a take-home question is bad. Some candidates might want to impress you and spend an unreasonable time on the question. Other candidates might ghost. (Ghosting means the candidate just stops responding to emails and phone calls.) The feedback loop just isn't there.

As a candidate, **when I get a bad whiteboarding question, it's easy for me to just muddle through it**. Depending on circumstances, I'll either decline the job or discuss the odd question with someone else in the process. **But, when I get a bad take-home question, I'm stuck between either ghosting, (not responding at all,) or providing feedback**. I've done both, and I'm not sure which is a better approach. The one case I gave feedback, the hiring manager got defensive, which confirmed my decision to walk away.

So, take-home versus whiteboard (or another live coding technique) isn't a slam-dunk decision. It's more about knowing what's critical and weighing tradeoffs. You can also just let the candidate decide for themselves, but then you have the overhead of planning two different kinds of interviews.

## How To Plan a Good Interview Question ##

In my experience, the purpose of a technical challenge is more than just determining competence. It's also how the interviewer verifies that a candidate has good comprehension of tasks, and can conduct themselves well during a technical discussion. Furthermore, these discussions ensure that the candidate can get along with the team.

A good question requires careful planning.

 1. Focus on 2-4 very high level programming concepts that are critical for the job.
 2. Have an emotional understanding of what "common sense" is for the job. (What kind of intuition should a candidate have?)
 3. Accept that you can't onboard someone in your entire stack in order to screen them
 4. What are specific biases you need to screen out?
 5. Are some concepts better screened in a live exercise or in a take-home assignment?
 6. Make sure that you're screening in people who will succeed in your questions.

Specifically, if you're trying to hire generalists, accept that new hires will have a learning curve. You won't be able to test such candidates on details of your stack because they probably haven't worked with it before.

In contrast, if you're trying to hire someone who can work in your stack from day one, it's okay for questions to require knowledge in your stack. You just need to make sure to only screen candidates who already know your stack. This is useful when screening contractors for short jobs.

Translation: **Only ask someone questions about frameworks and libraries if you require direct experience with those technologies. Otherwise, If you're interviewing someone with Postgres experience for a job with Oracle, don't ask Oracle-specific questions or assign Oracle-specific tasks.**

## You Can Ask the Candidate to Present Code they Wrote ##

One recent interview completely avoided a take-home question or a live coding exercise. Instead, they asked me to present some code of my choice. I spent some time going through my github repository finding some interesting work I've done over the past decade, and then we reviewed [a fitbit watch face][6]. Ironically, I probably spent 2-3 hours preparing; both in choosing the code to show, and then reviewing the watchface code as I hadn't looked at it in over a year.

The thing that I really like about this technique is that it's reusable. If another company asks me to present code, I'll just propose the fitbit watchface and it'll be fresh in my memory. Saves me time too!

## What Makes a Good Take-Home Question ##

**A good take-home question should take 2-3 hours.**

It's important to avoid planning fallacy where you think something is simple, but then what you thought should be 2-3 hours really is 15 hours.

What's critical is to avoid pushing a learning curve on a candidate. **If your take-home require specific libraries, languages, frameworks, ect, then you should discuss it with the candidate before you send the question.** Any framework or tool that you ask the candidate to use should be on the candidate's resume.

Dave Rolsky, in [A Technical Hiring Process Revisited][7], says:

> When you come up with a homework exercise you must have at least two (and preferably more) of your existing staff do it, including some people who were not involved in formulating the exercise.

> If the homework is in a problem domain that relates to what your company does (and it should be) then you should expect in house to staff to finish it in half the time it will take a candidate.

**In my experience, the screenings that I like are "use any language, libraries, and frameworks" and then run in a shell; or verified with unit tests; or present a very basic HTTP API.** You can also direct the candidate to use a language that they claim significant experience in.

My favorite assignment was for me to feel my way through a maze that was provided incrementally though a REST API. Another one had me write a RAM database and tests.

When I was desktop client architect at [Syncplicity][8], I occasionally used a [coding challenge][9] that had candidates build a document by loading multiple XML documents from a web server. We primarily screened candidates with C# experience for a job in C#, so we asked the candidate to use C#. But, when we interviewed a Mac developer with Objective C experience, we asked the candidate to use Objective C; and if we interviewed someone who needed to learn C# on the job, we'd let the candidate use their most comfortable language. We assumed that candidates could load documents over HTTP and work with XML. Internally, we had a few engineers make sure they could do the exercise in about an hour or so.

We did not require any specific library or framework, but we did suggest two of .Net's built in XML parsers as a hint.

Working on the Syncplicity desktop client often required working with poorly documented APIs, so we deliberately kept a little bit of ambiguity in the instructions. My [original version of the challenge][10] was slightly more ambiguous, and I think "more fun" for people who can work with poorly documented APIs.

## How To Plan a Good Whiteboard Question ##

A good whiteboard question requires careful planning. The interviewer, or team, should be able to plan a 1-hour interview in about 1-2 hours, but can re-use the interview for most candidates, and even different (but similar) positions.

 1. Focus on 2-4 very high level programming concepts that are critical for the job.
 2. Have an emotional understanding of what "common sense" is for the job.
 3. Plan coding questions that rely on 10-20 lines of very basic code.
 4. Plan the discussion: How do you give instructions? (What's written and what's verbal?) Assume you're going to have to explain all needed APIs and algorithms.
 5. Plan hints
 6. Anticipate biases that indicate a poor match
 7. Decide if these are true whiteboarding questions, or if the candidate will bring a laptop, or if you'll do these in teleconference with an online coding tool

**I personally like to include a question that tests a candidate's listening skills,** where the task just requires following instructions. These aren't trick questions, instead, **they're meant to ensure that the candidate can participate in design discussions and comprehend assigned tasks.**

**I also include a very straightforward question where I provide an API and have the candidate transform data from one format to another format.** These primarily filter out candidates who don't get certain basic concepts; but I also use these to filter out candidates who's fundamental biases would mean they wouldn't fit in with our team, or demand that we refactor everything because we didn't use their framework or design pattern of choice.

**As a candidate, the thing that I love about live coding exercises is that they give me a real good feel for what it's like to work with someone, and what the general team dynamics are.** In the case when the interviewer slammed me for not microoptimizing my code, I realized I didn't want to work there. It was more due to their attitude than the desire to microoptimize.

**What if it's important that a candidate demonstrate using an algorithm?** Actually, that makes a great live coding question. **Just make sure that the question involves explaining the algorithm instead of expecting the candidate to know it ahead of time. Or, before the interview, suggest that the candidate review the Wikipedia page about the algorithm.**

## In Summary: It's about planning a good interview ##

When I'm a job candidate, I really don't care if a coding question is whiteboard or take-home; nor do I care if it's truly on a whiteboard, or I need to bring my laptop, or we do a teleconference with an interview website. I like the style where I can present some code that I've written, but it's certainly not required for me.

Some candidates have preferences, and if you're in a situation where you want to please everyone, the best thing to do is let the candidate choose between some form of live coding, take home, or presenting existing code. Assuming you don't give the candidate the choice, just look at the tradeoffs and pick what's more important to you.

**What's important is that you research how to plan good questions and make sure that everyone involved in the process is also trained in running a good interview.**

<table>
<tr><th>Take Home</th><th>Live (Whiteboarding)</th></tr>
<tr><td>Some candidates feel less pressure</td><td>Other candidates want team interaction</td></tr>
<tr><td>Can evaluate more complicated code</td><td>Can evaluate collaboration and behavioral style</td></tr>
<tr><td>May not get feedback on issues with the question</td><td>Can get feedback on the question from body language, tone of voice, and reactions</td></tr>
<tr><td>Candidate can use the tools and environment where they are comfortable</td><td>Coding on a whiteboard or in an online tool is about discussion</td></tr>
<tr><td>Candidate has little time pressure</td><td>Candidate has time pressure</td></tr>
<tr><td>You evaluate the finished product as a whole</td><td>Insight into the candidate's thought process</td></tr>
<tr><td>All hints have to sent ahead of time</td><td>You can give hints on a case-by-case basis</td></tr>
<tr><td>Can not adjust the question as you go</td><td>Can adjust the question as you go</td></tr>
<tr><td>Easy to send a take-home to a "maybe" candidate</td><td>Company has to invest extra time for each "maybe" candidate</td></tr>
<tr><td>Can waste a "maybe" candidate's time</td><td>Company has to choose wisely before inviting a "maybe" candidate</td></tr>
</table>

  [1]: https://news.ycombinator.com/item?id=22828808
  [2]: https://www.freecodecamp.org/news/why-is-hiring-broken-it-starts-at-the-whiteboard-34b088e5a5db/
  [3]: https://www.joelonsoftware.com/2007/06/05/smart-and-gets-things-done/
  [4]: https://leetcode.com/
  [5]: https://coderpad.io/
  [6]: https://github.com/GWBasic/Binaryish-Clock
  [7]: https://blog.urth.org/2019/07/11/a-technical-hiring-process-revisited/
  [8]: https://www.syncplicity.com/
  [9]: https://andrewrondeau.com/Challenge2/
  [10]: https://andrewrondeau.com/Challenge/
  