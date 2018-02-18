---
title: "Go Microservice Example"
date: 2018-02-16T12:59:21+01:00
tags: ["Go", "Microservices"]
categories: ["Blog"]
draft: true
description: Presumably a best-practice example of a microservice in GO
---


## Intro

As did to find a good example of how to write a microservice in go including all
the stuff around it, like build infrastracture, test setup deployment handling,
I rolled one myself. Maybe this is even helpful for others trying somehting
similar.

## Goals

* testability by design
* reasonable usage of existing frameworks: rather provide a good example of concepts  than yet another application of framework xy
* CI capable build / test setup
* easy to read, easy to adopt
* adress some cross-cutting concerns like 
* DO NOT provide a solution for everyone, but provide a good starting point 


## Design


