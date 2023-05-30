---
title: "An Introduction to Learning Theory"
date: 
tags: [learning-theory, tcs]
category: [pedagogy]
draft: true
---

# Introduction

Let's say we have black-box/oracle access to a Boolean function $f$ on $n$ variables $f:\{0,1\}^{n}\xrightarrow{}\{0,1\}$. We are also given a set $S$ **labeled examples** in the form of $n$-bit strings (*read:* instances) and the output of $f$ on each such string (*read:* labels).

> **Our goal:** Find a *good approximation* $f^\prime$ of $f$ *efficiently*.


There's a lot to unpack here. We have to rigorously define what we mean by
- good approximation
- efficient

To put this in context, lets start with a small example. I like to use my favorite analogy: Pizzas and Taste.



