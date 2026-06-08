---
layout: post
title: "We Code to Understand"
date: 2026-06-08 00:00:00 +0900
categories: [Education]
tags: [Computational Thinking, Computational Essay, Wolfram Language, STEM]
description: "How shifting from mathematical to computational thinking — and from consuming to producing knowledge — is reshaping STEM education."
author: Junseo Lee
math: false
published: true
---

Formal thinking, the cognitive ability to reason abstractly and logically, is one of the most important aspects of human reasoning. For a long time, the central form this took was mathematical thinking, which involves formulating real-world problems as mathematical expressions and manipulating them symbolically. In this tradition, we learn to set up equations, prove theorems, and make estimations, which is certainly foundational, but no longer sufficient.

With the rise of advanced computing and artificial intelligence, the center of formal thinking has shifted. We increasingly need to think computationally, not just mathematically, because across STEM, many questions worth asking have outgrown what pen-and-paper manual methods can possibly reach. For example, large real-world datasets cannot be manipulated by hand, and large-scale simulations cannot be run by hand. Computation is what closes that gap. Therefore, computation is becoming an important part of how disciplines operate, not an optional tool any longer.

> "Pick any field X, from archeology to zoology. There either is now a 'computational X' or there soon will be."
> — Stephen Wolfram, [*How to Teach Computational Thinking*](https://writings.stephenwolfram.com/2016/09/how-to-teach-computational-thinking/)

If computation has become this essential, one would expect the secondary and undergraduate STEM curricula to incorporate it as an essential part while giving students structured opportunities to practice computational thinking. I assert they largely do not because while they do teach coding, traditional programming coursework mostly stresses getting familiar with the programming language syntax and understanding existing algorithms rather than computational thinking that leads to unprompted, original explorations driven by interests.

Thinking about this in terms of [Bloom's taxonomy](https://en.wikipedia.org/wiki/Bloom%27s_taxonomy), most STEM coursework involving computation operates at the lower cognitive levels: *remembering*, *understanding*, and *applying*. Students memorize programming concepts and syntax, and learn to apply them to textbook problems. This is absolutely essential, but it is not sufficient for students to think computationally. What is largely absent is the upper end: *analyzing*, *evaluating*, and *creating*.

![Bloom's Taxonomy pyramid](/assets/images/posts/wcrc-figure1.png)
*(Image Source: [Wikipedia](https://en.wikipedia.org/wiki/Bloom%27s_taxonomy))*

Thus, the act of identifying one's own question, investigating it computationally, and communicating findings as an original intellectual narrative is what most STEM curricula do not train. Students *consume* knowledge, and they rarely *produce* it.

This is the gap the program at the [Wolfram Computational Research Club (WCRC)](https://wolframclub.notion.site/homepage) is built to close. The solution to the gap we identified earlier is to train students to write a computational essay using the Wolfram Language, which prompts a student to produce an original piece of work while improving their competence in computational thinking. The aim is not to learn to code, but *to code to learn*.

> "We code to understand."
> — Wolfram Computational Research Club

## The language built for thinking, not for implementation

An educational model of this kind only works if the tool properly supports it. The gap is further compounded by the conventional computational tools. As I said earlier, traditionally, programming education has mostly focused on learning how to *instruct* a machine to perform a task by telling it what to do at each step, which we call an algorithm. This is because most widely used programming languages such as Java, Python and C++ require a user to care about what happens at the low-level implementation before they could start applying the computational paradigm to real-world problems. This consequently makes the gap between having the idea and computing it using the higher-level cognitive abilities (*analyzing*, *evaluating*, and *creating*) even bigger.

> "Programming—and programming education—have traditionally been about telling a computer at a low level what to do. But thanks to all the technology we've built in the Wolfram Language, one doesn't have to do that any more. One can express things at a much higher level—so one can concentrate on computational thinking, not mere programming."
> — Stephen Wolfram, [*How to Teach Computational Thinking*](https://writings.stephenwolfram.com/2016/09/how-to-teach-computational-thinking/)

The low-level implementations consume cognitive effort that should go toward disciplinary problem-solving. For example, if a statistics student has an excellent idea about analyzing data, and getting from the idea to a computed result requires many hours of algorithm design, implementation, and debugging, the tool has become the obstacle. Therefore, the distance between having an idea and computing it becomes prohibitively large when the tool does not express ideas at a high level.

[The Wolfram Language](https://reference.wolfram.com/language/) is designed to make that distance small. Stephen Wolfram has argued for the distinction between a programming language and a computational language. *A programming language* tells a computer what to do. *A computational language* is for expressing computational ideas, in a form both a human can understand and a computer can process. The distinction is that the former is a tool for managing machines while the latter is a tool for thinking and communicating ideas.

> "It's not just a language for telling computers what to do. It's a language that both computers and humans can use to represent computational ways of thinking about things."
> — Stephen Wolfram, [*What We've Built Is a Computational Language (and That's Very Important!)*](https://writings.stephenwolfram.com/2019/05/what-weve-built-is-a-computational-language-and-thats-very-important/)

What makes this practical is that [the Wolfram Language](https://reference.wolfram.com/language/) is knowledge-based and multi-paradigm. Thousands of algorithms and large bodies of curated real-world data are built directly into the language. Everything, whether data, code, or graphics, is a symbolic expression that can be utilized through a single function call. Students express ideas at the level of their discipline, not at the level of implementation. You do not import a library to do linear algebra or symbolic math. They are both part of the language. Furthermore, being a computational language understood by both humans and machines, the language is very readable as most function names are composed of standard English words rather than jargony abbreviations that are difficult to understand at first glance.

![Wolfram Language overview](/assets/images/posts/wcrc-figure3.png)
*(Image Source: [Wolfram Research](https://www.wolfram.com/language/))*

This matters not as a matter of convenience but as a requirement for the pedagogy. Without it, the program's target, moving a student from zero experience to a published computational essay in a single semester, would not be realistic for non-CS STEM students. With the power of the Wolfram Language, the cognitive effort can go where it should: on the research question, the exploration, and the argument the essay is making.

## The Computational Essay

A [computational essay](https://www.wolframcloud.com/obj/Expositions/Published/ComputationalEssayGuidelines) is an interactive narrative that weaves human language, code, and computer output to communicate an idea. Stephen Wolfram describes it as ["an intellectual story told through collaboration between a human author and a computer."](https://writings.stephenwolfram.com/2017/11/what-is-a-computational-essay/) It is not a coding notebook with prose around it, and it is not an essay with code attached. The text, the code, and the computer outputs are all combined together to construct a coherent argument or narrative.

The essay interleaves four elements:

1. *Ordinary text* expresses the main idea in a few sentences.
2. A short description of the code, what we call *code text*, previews what the code does.
3. The *code* itself, written in the Wolfram Language, gives a precise specification of the ideas being discussed. At the same time, it is an input for the computer to process.
4. The *output*, often graphical, delivers the result in a form that is intuitive and interactive.

The text sets up the idea, the description previews the computation, the code specifies it precisely in a manner that both a human can understand and a computer can process, and the output delivers the computed results.

![Anatomy of a computational essay](/assets/images/posts/wcrc-figure2.png)
*(Image Source: [Wolfram Research](https://www.wolframcloud.com/obj/Expositions/Published/ComputationalEssayGuidelines))*

This structure does something important pedagogically. The code must produce results that fit the narrative. You write a paragraph explaining what you expect. You write code to test it, and the code runs and produces output. As a result, either the output matches what you described, or it does not. Students cannot hide behind vague reasoning, because the computation either supports the argument or contradicts it. The medium itself forces precision and critical thinking.

> "The ordinary text gives context and motivation. The computer input gives a precise specification of what's being talked about. And then the computer output delivers facts and results, often in graphical form. It's a powerful form of exposition that combines computational thinking on the part of the human author with computational knowledge and computational processing from the computer."
> — Stephen Wolfram, [*What Is a Computational Essay?*](https://writings.stephenwolfram.com/2017/11/what-is-a-computational-essay/)

Writing a computational essay is therefore an act of **deep understanding**, that involves *analyzing*, *evaluating*, and *creating*, rather than a mere presentation or application of finished knowledge earned through reading a textbook or listening to lectures. If you understand your topic, your prose, your code, and your output must align. If you do not, the misalignment tells you exactly where the gap exists. This is what makes the computational essay a particularly effective medium for STEM learning. So, by writing a computational essay, students gradually move up in Bloom's taxonomy.

Numerous examples of computational essays can be found on [Wolfram Community's Staff Picks](https://community.wolfram.com/content?curTag=staff%20picks). The topics span a wide range of fields, but they share the same form: a genuine question, computational investigation, and a coherent intellectual narrative.

## Wolfram Computational Research Club

I founded the [Wolfram Computational Research Club](https://wolframclub.notion.site/homepage) (WCRC) at SUNY Korea in Fall 2025 to put this model into practice. WCRC is a research-oriented and faculty-advised club, with the purpose of cultivating computational thinking and strengthening technical communication through writing computational essays in the Wolfram Language.

> "We are not a coding club, we are aspiring computational researchers."
> — Wolfram Computational Research Club

As of Spring 2026, the club has 14 students from four different fields of study, four faculty advisors from the Applied Mathematics and Statistics and Computer Science departments, and [11 computational essays](https://wolframclub.notion.site/clubgallery) have been published on Wolfram Community in total.

The heart of the program is a four-phase workshop structure. In **Phase 1**, members explore topics and conduct an initial literature review across all groups together. **Phase 2** moves into research proposals and structured outlines, group-wise and advisor-led. **Phase 3** is two rounds of feedback, first on a complete draft and then on a revision, again with advisors. **Phase 4** closes with final review and publication.

![WCRC four-phase workshop structure](/assets/images/posts/wcrc-figure4.png)

The [Intro to Wolfram Language Crash Course](https://wolframclub.notion.site/intro-to-wolfram-language?source=copy_link) runs in parallel with Phase 1, so members build language proficiency while they explore topics.

![Intro to Wolfram Language Crash Course](/assets/images/posts/wcrc-figure5.png)

The semester closes with a [Computational Essay Showcase](https://wolframclub.notion.site/computational-essay-showcase?source=copy_link) where each member presents a 10-minute talk open to the campus community. This is the moment where the work becomes public and members present themselves as who they are — **computational researchers**.

## Implications

The computational essay is a transferable educational model; it is not limited to our club's program. Any STEM program with access to the Wolfram Language could adapt this structure, and any program serious about moving students from consuming knowledge to producing it should consider doing so.

The choice of tool, in this kind of work, is a pedagogical decision and not a technical one. A language designed for thinking produces different educational outcomes than a language designed for implementation. It is what makes it possible to guide a student with zero experience from an initial idea to a published essay in one semester.

The distance between having an idea and computing it should be as small as possible. The Wolfram Language makes that distance small. The computational essay gives students a motivation to cross it. And the scaffolded program structure guided by Wolfram Language experts and faculty advisors with domain expertise ensures that the students can produce meaningful original works.
