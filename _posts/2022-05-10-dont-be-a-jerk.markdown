---
layout: post
title:  "Don't be a jerk"
date:   2022-05-10 22:00:00 +0100
category: stories
tags: work wisdom
---

A few months ago I helped hiring Bob, a young software engineer with some of the skills the team
lacked. Bob is often silent, but when he speaks he often says sensible things.

Bob loves clarity and precision, so he is quick to point out inefficiencies and often suggests
improvements.

The team listens to Bob's suggestions and adopts them with an open mind.

In example:

- The team adopts Technical Decision Logs to document important technical choices because Bob suggests
  that the current documentation is not structured enough.
- The team adopts Jira because Bob suggests that planning is not efficient/detailed enough.
- The team implements some new features with serverless because Bob suggests that it's so much
  better than anything else.

Although the team doesn't blindly buy into Bob's ideas, at least they want to try them.

But Bob is not satisfied.

Whenever the team adopts one of Bob's suggestions, he criticises how that was done.
Whenever the team improves one aspect, Bob finds three new things to change.

Bob is never satisfied.

The team politely explains to Bob that some of the problems he wants to tackle are rather trivial for
the business and don't bring any value to customers.
Passive-aggressive criticisms become softly-spoken rants.

One day Bob casually questions the team's decision to use Clojure, claiming that it's too niche of a
language and we will never be able to hire enough engineers.
I mention that Clojure is a good language to solve most of the problems we deal with, although I
concede that we decided to use Python for Machine Learning (Bob doesn't like Python either).

I ask Bob why he is so categorical about Clojure. Bob says that Clojure code is very hard to
understand, and even harder to test.
Words fight to burst from my mouth, they clog my throat, but I somehow manage to stay calm and remember
that my first impact with functional programming was mind-bending. Pleasantly mind-bending, yet not
effortless. I see where Bob is coming from and sympathise with him.

I mention my experience to Bob and observe that, since his background is in C#, some mental shift can
be required to move from an imperative to a functional paradigm: that's totally fine, I went through
that myself, take your time and enjoy the ride.
I also mention that Clojure is possibly the most-loved language, commonly adopted by highly experienced
developers, many of whom use it for banking and financial applications: there must be a reason for that.

Bob is skeptical, he defines learning Clojure as a professional suicide.

I want to ask him why he ever decided to join the team, since he knew that Clojure was on the table,
but choose to ask something different instead: I ask what language he would use to do what we have to do.
Bob thinks for a while, then simply replies "I don't know".

Bob, the super-opinionated fixer of the world's problems, doesn't know.

A couple of days later, the CTO tells me that Bob has decided to leave.
He joined the team three months ago.

A thought surfaces in my mind: __"Don't be a jerk"__.
That's an advice from the book _That's Not How Much They Pay. That's How Much They Pay You_.
A powerful, pragmatical advice.

Bob has been a jerk.

He was given trust, yet he betrayed it by criticising the team's efforts to try his advices.
He was given freedom, yet he used it to spread negativity in the team.
He was listened to, yet he assumed that he always knows better.

Bob did the only thing he was able to do: try to reproduce the only way of working he knew,
regardless of the specific needs of the new environment he was in.

Bob's notice period is one week, he will be gone by Friday with little time for a proper handover.
What is Bob leaving behind? Technical debt, I'm finding out.

Bob advocated meticulous documentation, yet hasn't documented nearly anything of what he did.
Bob pushed to capture all our infrastructure in Terraform, yet he snowflaked many things without
telling anyone.

What did I learn from all this?

- Bob is actually not a jerk, just a victim of his own ego.
- I should have worked with Bob more closely before trusting him so much.
- Hiring is hard because people's ego can hide in subtle ways, becoming invisible until it
  feels safe enough to detonate.
- Empathy is a magic lens that can help revealing invisible things.

