---
layout: post
title: Plugin Architecture using .NET
subtitle: Coming up
---
A plugin architecture is an architecture that will call external code at certain points without knowing all the details of that code in advance. Plugin architecture is the manifestation of a development principle called Open Closed Principle (OCP), which states that the system must be open for extension but closed for changes. The reason a system must be open for extension is obvious that the requirements change and the reason why system should be closed for change is the fact that most of the bugs are added when changing an old code that we don’t truly understand about.

Okay! Wait! But why do we need plugin architecture?

With plugin architecture
* You can worry about the core funtionality of what you are building as a plugin
* No referencing problems within a big project
* version conflicts
