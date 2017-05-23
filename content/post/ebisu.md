+++
banner = "ebisu/math.jpg"
categories = []
date = "2017-04-28T04:48:19-04:00"
description = ""
images = []
menu = ""
tags = ["tech"]
title = "Announcing: Ebisu and Ebisu.js"
draft = false

+++

I am very pleased to announce the release of [Ebisu](https://fasiha.github.io/ebisu/) and [Ebisu.js](https://fasiha.github.io/ebisu.js/)!

These are are public-domain software libraries intended to be used by quiz or flashcard apps for intelligent, Bayesian quiz scheduling. They are released under the completely free [Unlicense](http://unlicense.org/), and can be used anywhere by anyone without any conditions 🎆. The underlying algorithm is based on a principled application of Bayesian statistics, and has many nice features compared to existing scheduling algorithms 😁.

[**Ebisu**](https://fasiha.github.io/ebisu/) is the reference Python implementation (available on [PyPI](https://pypi.python.org/pypi/ebisu/)) that is accompanied by detailed mathematical exposition, cool plots, and unit tests that check the math against alternative implementations.

[**Ebisu.js**](https://fasiha.github.io/ebisu.js/) is the JavaScript port. This link has fewer details but does include a couple of interactive visual demos to help understand what the algorithm is doing under the hood—though this understanding is purely educational.

Quiz app authors can use either library, or port the algorithm to another language, and use it as a black box module for handling quiz scheduling. You don’t need a deep understanding of Bayesian statistics or the underlying statistics, but, if you *are* curious 🤓: the scheduling algorithm places a probability distribution on the recall probability at one specific time, and propagates this distribution through time. In this way, it can *predict* at any given time the recall probability for any fact that a student has already learned. The algorithm can then *update* its belief about the recall probability when the student passes or fails a quiz.

All this means you’re not limited to any minimum interval, like one day in Anki—the algorithm can predict a 95% recall probability for one fact, 90% for a second, and 26% for the third, so of course the quiz app should test the student on the third fact. Then, if the student wants to continue reviewing, they can be tested on the second fact (with 90% recall probability). As this demonstrates, Ebisu can provide on-demand estimates of recall probability for any learned fact, and gracefully handles over-reviewing.

It also handles under-reviewing just fine: there’s no concept of “due date” in Ebisu, just recall probabilities, which are ordinary numbers between 0 (sure to fail) and 1 (sure to pass). If a student skips studying for a few days, each fact still gets a recall probability, which you can sort and review those facts most in danger of being forgotten.

As an extra bonus, Ebisu also elegantly handles “passive reviews”, i.e., those situations where the student is just shown a fact without actively having to provide an answer, and there is no “pass” or “fail”. This is nice because many students enjoy passive review, but also because it enables a new workflow for reviewing facts with sub-facts.

For example, a Japanese fact is that the kanji 人 is pronounced “ひと” (hi to) and means “person”. A quiz app can break this fact into three sub-facts, corresponding to three different recall probabilities as well as three separate quizzes: (1) to elicit kanji 人 (maybe by drawing or by multiple-choice), (2) to elicit pronunciation ひと (maybe by typing), and (3) to elicit translation (maybe by multiple choice), each given the other two. When the student is asked to produce the kanji that is pronounced ひと and means “person”, and successfully produces 人, that student has *passively* reviewed the fact that 人 means “person” and is pronounced ひと—the kanji sub-fact can be updated with a pass active review, and the pronunciation and meaning sub-facts can be updated as passive reviews.

(Some quiz apps may choose to do this differently, e.g., model this entire fact as a *single* recall probability with Ebisu. Then, when this fact is reviewed, the app could choose which sub-fact to quiz on, at random or using [multi-armed bandit theory](http://stevehanov.ca/blog/index.php?id=132). A quiz app may choose the more complicated approach, i.e., using Ebisu to maintain a recall probability for *each* sub-fact and then updating these recall probabilities using both both active and passive reviews, in order to more accurately track student memory.)
