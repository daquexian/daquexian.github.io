---
layout: post

title: K-9 Mail GSoC Summary

date: 2017-08-29

categories: blog

tags: [Android, GSoC]

description: The summary of GSoC I participated this summer. It is the most valuable experience I have ever had.

---

I have contributed to [K-9 Mail](https://k9mail.github.io) in this summer as my [Google Summer of Code 2017 project](https://summerofcode.withgoogle.com/projects/#6430250689888256). It is so excited and honored I can take part in the process of development of K-9 Mail.

## Overview

K-9 Mail is a great mail app on Android. It is almost as old as Android system, and be loved by hackers for its unique and powerful features like PGP support. However as time goes by, there has been some missing features. During GSoC 2017, I focused on improving account setup under the guidance of Valodim.


## What I have done

The code I contributed is in [PR 2580](https://github.com/k9mail/k-9/pull/2580). It consists of following parts:

* Renew the UI of account setup, and rewrite them into MVP pattern.

* Autoconfiguration, incluing [Mozilla's autoconfiguration](https://developer.mozilla.org/en-US/docs/Mozilla/Thunderbird/Autoconfiguration), [RFC 6186](https://tools.ietf.org/html/rfc6186) and [Microsoft's autodiscover](https://technet.microsoft.com/en-us/library/bb124251(v=exchg.160).aspx)

* OAuth 2.0, including Gmail and Outlook(hotmail), so that Gmail can be accessed without allowing "less secure apps" in gmail.


## Skills Developed

During GSoC, I had a wonderful time with my mentor (I also met a friend, the other student in our org, rhari, too), and learned many knowledge that hard to learn if I develop something by my own:

1. I wrote unit tests for the first time. When I writing my own projects before, I had never written unit tests, and seldom heard them. It is because my own projects is small and like a toy anyway. I learned how to write unit tests in the development of K-9, what is "test-driven" development, and also the importance of unit tests. (I'm already planning supplement unit test in [my own project](https://github.com/daquexian/chaoli-forum-for-android-2/issues/17) :) )

2. I had a better grasp of MVP pattern. Actually I have a own app that is written in MVVM pattern. However my thought of these patterns is not exactly right, especially on the separation of concerns between view and presenter. Under the guidance of my mentor, I learned what is the key of MVP pattern.

3. I learned what oauth 2.0 is and how it works. The support of oauth 2.0 of Gmail and hotmail is one of the most exciting things I have done in GSoC. 

## What I have learned

* ### Communication is IMPORTANT. 

    My mentor is very responsive on Riot to me and another student, rhari. But I have to admitted that I didn't spend my all time on the task of GSoC, because I was distracted with many things. Due to the lack of my communication, it caused trouble for both my mentor and me. It taught me a lesson that I should never be shy to express the troubles I met.



## The future work

However I have not finished all the work listed in my proposal. I'm ashamed that I didn't do enough things in GSoC. I'd like to continue to contribute to K-9 after GSoC ending. Here is the future work to be done:

* Using both client certificates and password auth

* User friendly certificate error handling

* Certificate viewing and pinning

* User friendly TLS / SSL error handling

## Acknowledgement

Finally, I would like to express my GREAT thank to my mentor, Valodim, and all other hackers in K-9 team, namely cketti and philips. The experience of GSoC is <del>one of</del> my best and valuable experience in my whole life. I'm very very happy to meet these excellent hackers. I will continue to contribute to K-9. Thanks :)
