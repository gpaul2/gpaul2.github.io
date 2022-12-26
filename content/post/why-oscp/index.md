+++
author = "Geoji Paul"
categories = ["Security"]
date = 2020-05-31T23:13:33Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1489533119213-66a5cd877091?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ"
slug = "why-oscp"
summary = "Reasons for pursuing OSCP may vary from person to person. However, it is important to think of those reasons and understand if it justifies the effort. "
tags = ["Security"]
title = "Why OSCP?"

+++


This post is a prequel to my earlier post  _[From newbie to OSCP in 100 days](https://www.geoji-paul.com/post/from-newbie-to-oscp-in-100-days/) _ and in this post, I will go into the rationale behind why I went after OSCP certification

_**TL;DR:** Reasons for pursuing OSCP may vary from person to person. However, it is important to think of those reasons and understand if it justifies the effort. For me, the reasons were to learn to think like an attacker, experience real world security gaps, and finally to build confidence around learning and applying new skills._

Before going into the reasons, let me give a brief background of where I was in my info sec career/learning journey when I decided to go after this certification. In early 2018, I was in, what might be called as the middle management (still is as of writing this blog) in the Information Security department at a large corporation. At that point, I already had a CISSP and CISM certification and was leading multiple information security functions.

May people asked me "why OSCP?" when I decided to pursue it. People who went through it warned me of the time-commitment and potential for failure. And they all were asking the right questions, and were accurate in warning me of the things to come. And yet I decided to pursue it. And it was for these reasons.

### Learn the '_attacker mindset_'

This perhaps is the biggest reason why I decided to pursue the certification. Even after having "sufficient" knowledge about the security domain, I often found myself struggling to understand why a certain vulnerability or why a certain information sharing could be exploited by an attacker. After all, we have firewalls, IDS, IPS, 'you name it and we have it' kind of arsenal in defensive tools. How could then an attacker be able to get in and why should I be concerned?

To give an example, as all security people do (and usually to their detriment), I once got into a debate with a penetration tester about sharing employee login IDs with a third party. At the time, I argued that sharing of login ID, although sensitive information was not very risky due to two factor authentication. How could an attacker defeat 2FA? Why was the penetration tester insisting that it was riskier than my assessment? Although I understood the argument, I did not comprehend it. Looking back at that incident now, I realize the flaws in my assessment. This change in perspective occurred as a direct result of my time spent in OSCP labs. I used many online brute forcing tools in the labs and I _started to think like an attacker_ on how 2FA could be subverted.

That was just one example of the many where my perspective changed after going through the labs. And that change in perspective, to always doubt the strength of defensive controls and to think how an attacker might defeat it have proved to be of tremendous value when assessing risk, building solutions or just in general on educating others of the gravity of situation with certain gaps.

### Experience 'real world' effect of security gaps

When I explained to someone about the need to gain attacker mindset as the main reason to pursue OSCP, they asked me why I wasn't going after Capture The Flags (CTF) as a way to obtain that skill set. I have to admit that I did not have a good answer to that question at the time it was posed to me. However, I did have a hunch that CTFs might be a bit too artificial compared to OSCP. And that turned out to be true after having participated in a few CTFs during my OSCP prep.

I do not want to leave the impression that CTFs are not useful in developing an attacker mindset. CTFs are great and I love participating in it. It makes you think creatively and inquisitively to accomplish your goal. It teaches you to be persistent and search in all possible places where you could find a flag to get to the next level. And those are very similar skills to what one will learn from OSCP. However, the subtle difference between a seasoned hacker and a seasoned CTF player in my opinion is around finding the path of least resistance. When you are participating in a CTF, your thoughts are fine-tuned to think about where the CTF creator would have left a clue. However, when you are working through the OSCP labs, your thoughts must be fin-tuned to think about what a system administrator or a developer might have done insecurely.

A different way to explain the above is that, in the OSCP labs, you are not going to find a secret message hidden in the source file of a web page (perhaps with the exception of one lab machine) or a hidden text in an image file. However, you will find common mistakes made by system admins or developers when they administer systems or develop code. And the focus is in finding those issues as opposed to hidden clues.

Now, I understand that one could argue that the differences pointed out above are very minimal and that I am splitting hairs. I could be splitting hairs, but my personal experience is that participating in CTFs actually did not help me prepare much for OSCP. In fact it had the detrimental effect of over complicating or overthinking about challenges when the next step was often just in front of me. I just had to think about what a lazy or inept system admin would do.

### Confidence Boost

Last, but not the least of the reasons why I pursued OSCP was because I needed a confidence boost. Having developed software for many years and then detaching from technology for managerial roles to eventually come back to technology and security left me at a place where I was overwhelmed by how technology had evolved in the few short years, and not to mention the advancements in information technology during that time. I needed to get back into technology and I needed to prove to myself that I could be the technologist that I used to be. And OSCP was just the right pick since I knew it would challenge me as well as help me learn about technology that I did not know before. That said, it is important to keep in mind that OSCP is not the cutting edge or bleeding edge in technology. In fact, much of the content were using tech that was around for many years. However, a specific technology learning was not necessarily the learning I wanted to accomplish. It was rather in gaining the confidence of being able to learn quickly and use technology that I did not know previously.

### Summary

Each person is different and their motives are different. My reasons for pursuing OSCP might be very different compared to yours. If you are an info sec professional looking to gain attacker mindset, learn exploiting of real-world security gaps, or perhaps to build the confidence for solving technical challenges, OSCP will be a good fit for you.

