+++
author = "Geoji Paul"
categories = ["Security"]
date = 2020-03-08T01:21:16Z
description = ""
draft = false
featured_image = "https://images.unsplash.com/photo-1533709752211-118fcaf03312?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ"
slug = "from-newbie-to-oscp-in-100-days"
summary = "This is yet another OSCP story among the many other stories that are out there. It captures my journey from being a newbie to OSCP certified in roughly 100 days"
tags = ["Security"]
title = "From newbie to OSCP in 100 days"

+++


## Introduction

For those of you not familiar with the Offensive Security Certified Professional, a.k.a, OSCP, it is perhaps one of the most difficult security certifications to obtain. As the name implies, it is administered by the [Offensive Security Team](https://www.offensive-security.com/). It involves completing Offensive Security’s Penetration Testing with Kali Linux (PwK) course and passing a 24-hour hands-on exam. Passing the exam requires successful exploitation of multiple lab machines to reach a passing threshold and submission of an exam report within 24-hours immediately following the test. This article outlines my journey from being a newbie to OSCP certified in 100ish days.

## Background

Early on in my Information Security career, I had realized that an attacker mindset is necessary to become an effective defender. Although I understood the concepts of threats, vulnerabilities and controls, it was hard to envision how an attacker could exploit weaknesses to compromise systems or networks. In order to close that skill gap, I started looking into various options. I considered the Certified Ethical Hacker (CEH), but decided against it since my goal was to obtain hands-on knowledge, and SANS was not an option due to the high cost. As an alternative, I considered participating in various CTFs (Capture The Flag) to gain practical knowledge. But eventually decided against it since practical system exploitation rarely occurs through finding a ‘hidden clue in an image’ (more on it on a later post). After looking at various options, I decided that OSCP was the best available alternative and made it a personal goal for the year (2018). With that as the background, the real catalyst to building momentum came (ironically) as a CTF in April. This is when some of my colleagues put together a CTF for the Information Security team. Although it was a lot of fun and I managed to get a few flags, I walked out of there with a realization of how little I knew about Penetration Testing in general. I distinctly remember someone mentioning about Netcat, and it was the first time I had heard of it!

## The Journey to Mordor

Let me first start with what I meant by being a ‘newbie’. While it is true that I was a newbie to Penetration Testing, I did have a technical background. I was a full stack web developer for many years where I primarily developed Java based web applications. As part of the role, I had written SQL scripts, JavaScript and bash scripts apart from developing software in Java/J2EE(JEE).

The first 45 days of course work was probably the most tedious part of OSCP for me. During that period, I spent most of my weekends (or any days off) completing the coursework. But once I completed the coursework and started practicing in the lab, it soon transitioned to being fun and addictive. Perhaps as a function of ‘fun and addictiveness’, in the latter half, I spent roughly 3 hours most days during the week and close to 12hrs over the weekends in preparation.

## The Course (45 days):

Thanks to my employer's training program, I was able to get lab access for 90 days. I signed up for the training and started active preparation around late July. I have to admit that I did not know what I was getting into when I started the journey. First of all, I was naïve in thinking that finishing the coursework would prepare me for the test. Little did I know that finishing the coursework was only a first step. That said, albeit unplanned, I completed the coursework in the first 45 days and used the remaining time practicing in the lab.

Something that I was adamant about when doing the coursework was to not skip any exercises. Although it did slow me down, I think the approach helped me to learn multiple ways to approach a problem. For example, one of the exercises early on in the course is to write a bash script for conducting a ping sweep in the lab. A second part of the same exercise is for doing the same using a higher-level scripting language. Although it would have been easy to ignore the second part and move on, I took time to learn Python and implement ping sweep in Python which helped me later on in the buffer overflow module.

The most difficult part of the coursework for me was the pivoting section. And it was for two reasons: #1, I did not have any prior networking experience and #2, the course materials are not very elaborate for the section. This is probably one section where I spent the most time outside of Offensive Security’s course material for self-learning. You will be tempted to skip the section, or skim the section due to the difficulty. I would encourage you not to fall for that trap since local and remote port forwarding comes in handy when you have locally listening services that needs exploiting for privilege escalation. After completing the coursework, the last section of the course walks you through a sample penetration testing scenario starting from scratch to owning all networks and domain admin. I found that very helpful since it gave me ideas on how to approach the lab machines.

{{< figure src="/post/from-newbie-to-oscp-in-100-days/gnome-shell-screenshot-J07CTZ.png" >}}I took extensive notes using KeepNote when doing the coursework. Each chapter had a dedicated folder with pages for every exercise. As mentioned earlier, I did not skip any exercise. Although it was time consuming, I completed and documented every exercise from the coursework. If I were to do this again, I will however look into applications that can generate PDF report directly from the notes. This is because, I had to spend a good amount of time copying contents from KeepNote to the lab report when I was getting ready to submit it.

## The Lab (45 days):

I wish I could say that I had a great strategy on how to approach the lab machines. The reality is that I did not have one. The first set of machines I targeted were the ones that could be used for pivoting into other sub-nets. I remember spending sometimes, up-to a week to get into one system. Did that help? Perhaps. Something that I learned in the initial days of playing in the lab with those hard to compromise (for a beginner) machines was to never give up. In the first 15 days of the lab, I compromised only four lab machines: **alice**, **sean**, **mario**, and **timeclock**. This was primarily because my goal was to get into all networks and some of these machines were not the easiest to begin with. However, I started picking up speed and got into 11 more machines during the next 15 days: **phoenix**, **bob**, **mike**, **barry**, **payday**, **ralph**, **pain**, **leftturn**, **luigi**, master and **jeff**. During this time I picked up ‘pain’ which is one of the big three (there are three notoriously hard machines and they are usually referred to as the big 3).

After the first month in the lab and having hacked into all networks, my goals were to hack as many machines as I could, increase speed, and get the rest of the big 3. Also, since I had scheduled my test right around the expiration of my lab, I was spending more time on the labs. Sometimes up to 3 hours a day. And before the test I picked up **gh0st**, **helpdesk**, **joe**, **mail** and **humble**.

## OSCP Attempt 1:

I had scheduled for the test on a Saturday. Before the test, I read through many OSCP stories and talked to a few people who had taken the test. Almost all recommended doing the buffer overflow machine while kicking off scans on the remaining machines to maximize the time. With that advice in mind, I decided to take the Friday off so I could practice the buffer overflow and organize my notes.

On the day of the test, I received the email with details of the exam lab connection promptly at time (8AM). I stuck to the plan and started with the buffer overflow machine. It was quite straightforward and I was able to get it in an hour. Next, I focused on a medium difficulty box. I was able to get a low privilege shell in the box by around 11AM. Before moving on to the next machine, I tried escalating privileges on that box for a couple hours without much success. Around 1PM, I decided to take lunch break. After lunch, I moved on to an easier box. After poking around for a couple hours, **success!** I was able to get root access into the machine by around 3PM.

After taking a quick break, I went back to the machine where I had the low privilege shell. I tried every privilege escalation exploit I knew without any success. I kept at it until about 8PM, chasing various rabbit holes for privilege escalation only to my detriment. At 8PM, I decided to take dinner break and moved on to another medium difficulty machine. I decided that I was going to use Metasploit (you are allowed to use Metasploit on one machine for the exam) against it and obtain a Meterpreter shell on the machine. I was pretty excited by now since I had gotten into 4 machines, including a Meterpreter shell on one of them, which I thought was going to give me an easy path forward for privilege escalation.

The excitement soon faded away since after trying almost every privilege escalation methodology, I was making no progress. Things started going downhill from there; I was getting tired and did not know what else to do to escalate privileges on these machines. Since I knew that escalating privileges on the two machines would give me enough passing points, I kept trying the same exploits again and again with no results. For a short period, I tried attacking the highest difficulty machine, but made no progress on it either. Finally, around 3AM, I decided that I was going to take some rest. I slept until 7AM, woke up and spent the last hour attempting privilege escalation on the boxes I had compromised. Nothing worked and my connection expired promptly at 7:45AM. **Failure!**

**Lessons learned from first failure:** The biggest lesson I learned from the first attempt was that my privilege escalation was a one trick pony. I was using only kernel-based exploits and did not know what else to look for. I realized that I needed to learn more ways to escalate privileges. Also, I learned that preparing the coursework related lab report (for extra points) on the day after the exam is not the best strategy.

## More Lab (15 days):

By now, I had invested too much time and energy into learning and I decided that I was going to give the OSCP one more try. I purchased 15 more days’ worth of lab time and focused primarily on privilege escalation. I went back into gh0st, humble and many other systems I had compromised earlier and escalated privileges differently from how I had done it before. Also, I compromised few additional machines during this time: **fc4**, **susie**, **hotline**, **core** and **sufferance**. In fact, I got sufferance the day before my second attempt and it was definitely a confidence booster!

## OSCP Attempt 2:

Since my second attempt was scheduled after Offensive Security introduced test proctoring, I got the proctored version of the test. For the proctored test, I had to login 15 minutes prior to setup the webcam and screen share. Thankfully, all proctoring formalities were completed within 15 minutes and I promptly started the test at 7AM CT. As with the last time, my strategy was to go after the Buffer Overflow machine as the scans were running on the other machines. This time, I completed the buffer overflow in less than an hour. I went after the easier machine after the buffer overflow and was able to get it within the next hour. After that, I moved on to a medium difficulty machine. I was able to get into it relatively quickly and within the next hour, I was able to escalate privileges on it too! By noon, I had three fully compromised machines.

Feeling relatively confident, I took a quick lunch break and started looking at the other machines. By around 3PM I got a low privilege shell on another medium difficulty machine. Since I had set a rule to work on a machine for no longer than 3hrs, I did not try escalating privileges on the machine other than some basic tests. So I moved on to the high difficulty machine. After working on it for a few hours, by around 7PM, I had a Meterpreter low privilege shell on the machine! By now, I had fully compromised three machines and partially compromised (low privilege shells) the remaining two. Although I knew that I might have enough passing points, I still decided to go the full length to escalate privileges on the last two as well.

At around 3AM, after verifying that I have all the screen shots and proof keys, I decided to stop the exam. After resting for few hours, I wrote the exam report and submitted it along with the lab report. Since I had already created the lab report with the previous attempt, the report submission was relatively quick. Everything was submitted by around 4PM and I received confirmation of my submission within a few hours.

## The Result

Since the Offensive Security published SLA for exam grading is 3 days, I was not expecting to hear anything until later in the week. However, it was a pleasant surprise to see their email the next day (after 26 hours) with a PASS result! It was exciting and very humbling to see that I passed. Excited because it was a hard-earned result and humbling because I knew that, as hard as it is, OSCP is only a beginning to the vast field of Penetration Testing. At least now I knew how an attacker thinks and I met the goal of what I set about to accomplish.

Overall, it was a worthwhile journey that taught me not only to think like an attacker, but to never give up!

{{< figure src="/post/from-newbie-to-oscp-in-100-days/IMG_1740.jpg" >}}



