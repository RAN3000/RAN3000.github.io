---
title: "Anki settings for flow."
date: 2020-09-12T18:42:56+02:00
comments: true
draft: false
---

I've heard somebody say that the only reason people go rock climbing is to get into flow state. Lately you can find me pretty often at the local cliff, I compared the statement to my own experience: I am getting into flow state while climbing and it's a big source of enjoyment (though it's not the only one).

Sometimes I get into flow state while going through my daily Anki review. Flow is a state we naturally desire, therefore it's something we can work on to make spaced repetition reviews emotionally more compelling. I'll explain some of the changes I've made to my Anki settings in order to maximize time in flow state while reviewing my flashcards.
<br/>I'll talk about spaced repetition for memorization. I think that memorization is just the tip of the iceberg, spaced repetition can aid deep understanding, it can trigger insights, thinking and reflection. These goals of spaced repetition are still rare, they are not well defined and ideas keep emerging, relating flow to them would be a much harder task, therefore for now I'll stick to the goal of memorizing information.

Mihaly Csikszentmihalyi describes the environmental conditions for flow in his book [Flow: The Psychology of Optimal Experience](https://www.amazon.com/Flow-Psychology-Experience-Perennial-Classics/dp/0061339202/ref=sr_1_1?dchild=1&keywords=flow&qid=1599916631&sr=8-1).
The conditions are:
- demand must match skill
- there needs to be a tight feedback loop with the environment (no ambiguous feedback)
- failure needs to matter

Anki reviews, with default settings and decent cards, sometimes meet those conditions. But I think we can do better.

## Tight feedback loop where failure is clear

People in the community always suggest to create atomic flashcards. I think that Michael Nielsen coined the term in [Augmenting Long-term Memory](http://augmentingcognition.com/ltm.html), if you are building a spaced repetition habit and have not read the article, you should stop whatever you are doing and go read it.
I want to make a couple arguments, from the point of view of flow, in support of atomic flashcards.

Let's say the flashcard is about a procedure with 2 steps. You'll need to remember: 
- that there are 2 steps
- the first step
- the second step
- which step comes first

The card should be marked as wrong if we didn't recall any of these concepts. We can break the card down by creating four cards, instead of just one, one for each of the concepts.
That way failure becomes much less ambiguous, for each atomic card we either get it right or we get it wrong, there's no partially right or partially wrong answers. 
<br/>In general complex cards can be hazy, we tend to leave out the details. Breaking down complex cards, that is atomizing them, forces us to focus on the details. We increase the resolution. Though we have to be careful to also retain the bigger picture, I like to keep also a few complex cards in my decks, but they constitute a very low percentage of them.
Doing this gets us closer to satisfying the one of the conditions of flow, tight feedback loop where failure is clear.

Atomizing our flashcards has a further advantage that the feedback loop gets even tighter. Generally it takes a shorter time to read the question and to answer in our minds, we have to retrieve from memory a single concept. We quickly reveal the true answer and thus get feedback: were we right or were we wrong? Either way we move to the next flashcard.
The same happens for complex flashcards but usually they are emotionally heavier and take longer time to answer, there's a risk of breaking our state of flow or of making it harder to get into it.

## Failure needs to matter

Writing atomic flashcard makes failure clear, now we need failure to matter. 
Anki's default settings do a good job with this, when we get a card wrong the corresponding interval is reset, we have to start anew. 
For a long time I only reduced the interval by some percentage, I didn't want to start over each time. I've now reverted this change in my decks' settings.
As a side note, I find it more effective to review the missed flashcard not after ten minutes, but after a day or two, I feel like my memory is better challenged.

## Skill must match demand

This is usually explained with a chart:

<img src="/assets/img/2020-09-12-anki-settings-for-flow/flow.jpeg" alt="Skill must match challenge for flow." width="250" vspace="20" hspace="5">

If the challenge is too hard for our skill level, we get stressed. If the challenge is too easy, we get bored. By overcoming a challenge, our skills improve and we can face harder challenges. Flow accelerates this process.

Card difficulty it's a tricky topic, there are multiple dimensions across which a cards can be more or less difficult.
Cards covering topics that come natural to us, feel easier. The way a card is written also plays a big role. Finally longer intervals between reviews make it more difficult to retrieve a card, we are more likely to have forgotten it.
<br/>We can immediately tackle this last dimension to get us closer to a match between skill and challenge: the spaced repetition community already suggest to target an accuracy of 80%-90% in reviews, which in Anki can be checked at the top of the *Stats* page.
<br/>I haven't found a way to act on the other dimensions, it's not clear to me which difficulty we should target, there are desirable difficulties and undesirable ones. For instance complex flashcards feels harder compared to atomic ones, but the latter should usually be preferred ((generally, see above) undesirable difficulty); some cards make me think hard in order to answer, usually if I'm successful I feel satisfaction, they are rewarding (desirable difficulty).
In general one idea could be to push easier flashcards at the start of the review session and harder flashcards at the end. So complex cards and cards we failed at multiple times should be left for later in the session. I can't think of a simple way to achieve this in Anki, moreover it's hard to mark cards with distinct kinds of difficulties.

## Conclusions

To summarize, by getting into a flow state in our spaced repetition review sessions, we can make those far more enjoyable. Some expedients to increase the chance of getting in flow are:
- prefer atomic flashcards
- reset intervals after failure
- tune intervals' length, so that reviews feel challenging, but not too much or too little

I've only recently started playing with this and paying more attention to how I feel while reviewing flashcards. I'd love to hear your personal experience and feelings about any of this. Any kind of criticism and feedback is very welcome, write to me at [giacomoran@gmail.com](mailto:giacomoran@gmail.com) or on Twitter [@randiisan](https://twitter.com/randiisan).
