---
title: lesson learned from log onboarding
date: 2024-06-22 01:31:00 Z
tags:
- detection engineering
- SIEM
layout: post
---

Some lesson learned from a log onboarding process:
* workstream block each other
* scope creeping
* unclear alert have minimums alert instead
* choosing the right log to onboard/ visibility
* threat modelling that focus on detection, a lot of threat modelling exercise result is hard to translate into detection.
* have a typical generic use case with requirement that should be on the log. have it group as STRIDE maybe? will be easier for user to see which one matches their cases.
* defined process and standards, QA, scope
* Inscrutable and unmaintainable detection content — if the detection was not developed in a structured and meaningful way, then both alert triage and further refinement of detection code will ..ahem … suffer (this wins the Understatement of the Year award) -> from chuvakin blog 