# Chapter 4. Evaluate AI Systems

A model is only useful if it works for its intended purposes. You need to evaluate models in the context of your application.  [Chapter 3](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#ch03a_evaluation_methodology_1730150757064067)  discusses different approaches to automatic evaluation. This chapter discusses how to use these approaches to evaluate models for your applications.

This chapter contains three parts. It starts with a discussion of the criteria you might use to evaluate your applications and how these criteria are defined and calculated. For example, many people worry about AI making up facts—how is factual consistency detected? How are domain-specific capabilities like math, science, reasoning, and summarization measured?

The second part focuses on model selection. Given an increasing number of foundation models to choose from, it can feel overwhelming to choose the right model for your application. Thousands of benchmarks have been introduced to evaluate these models along different criteria. Can these benchmarks be trusted? How do you select what benchmarks to use? How about public leaderboards that aggregate multiple benchmarks?

The model landscape is teeming with proprietary models and open source models. A question many teams will need to visit over and over again is whether to host their own models or to use a model API. This question has become more nuanced with the introduction of model API services built on top of open source models.

The last part discusses developing an evaluation pipeline that can guide the development of your application over time. This part brings together the techniques we’ve learned throughout the book to evaluate concrete applications.