---
date: 02-05-21
id: 16
path: Books
tags: []
title: Code Complete
type: note
---

# Introduction

Construction == design, coding, debugging, testing, etc. The central activity in development.

Software construction is the central activity in software development; construction is the only activity that’s guaranteed to happen on every project.
The main activities in construction are detailed design, coding, debugging, integration, and developer testing (unit testing and integration testing).
Other common terms for construction are “coding” and “programming.”
The quality of the construction substantially affects the quality of the software.
In the final analysis, your understanding of how to do construction determines how good a programmer you are, and that’s the subject of the rest of the book.

# Metaphors

Metaphors == models for understanding. Metaphors are heuristics, not algorithms.

Algorithm: well-defined set of instructions for a task.

Heuristic: technique that helps you look for an answer.

Metaphors:

* Penmanship/writing
* Farming/growing
* Accretion
* Building

Software construction requires a variety of techniques.

Metaphors are heuristics, not algorithms. As such, they tend to be a little sloppy.
Metaphors help you understand the software-development process by relating it  to other activities you already know about.
Some metaphors are better than others.
Treating software construction as similar to building construction suggests that careful preparation is needed and illuminates the difference between large and small projects.
Thinking of software-development practices as tools in an intellectual toolbox suggests further that every programmer has many tools and that no single tool is right for every job. Choosing the right tool for each problem is one key to being an effective programmer.
Metaphors are not mutually exclusive. Use the combination of metaphors that works best for you.

# Prerequisites

Good software results from using high quality practices, and emphasising quality at all parts of the process.

Prerequisites == preparation, planning, foundations, risk reduction, obtain requirements.

It is part of our job to educate the non-technical people about the development process.

What kind of software are we working on?

* Business systems
* Mission-critical systems
* Embedded life-critical systems

Depending on the kind of software, iterative or sequential development processes may be more appropriate. Most projects are a mix of both processes.

Sequential methods are suitable when:

* Requirements are stable
* Design is straightforward and well-understood
* Dev team is familiar with the application
* Low risk project
* Cost of changing requirements or design is high

Iterative methods are suitable when:

* Requirements not well understood
* Design is complex, challenging
* Dev team unfamiliar with the application
* Project contains more risk
* Cost of changing is low

Problem-definition prerequisite (also called product vision, vision statement, mission statement, product definition, etc). Defines the problem without reference to the solution(s). Helps to prevent wastage by solving the wrong problem.

Requirements prerequisite are an explicit set of requirements from the user. The user can review and agree to them. Prevents guessing and arguments. Helps to minimise changes after work begins. Requirements always change (average 25%), but the aim is to minimise this.

Architecture prerequisites (also called system architecture, high-level design). Typical components include program organisation, major classes, data design, business rules, UI design, resource management, security, performance, scalability, interoperability, internationalisation, I/O, error processing, fault tolerance, feasibility, over-engineering, buy-vs-build, reuse decisions, change strategy.

Allow 20-30% of schedule to prerequisites planning (not inc. detailed design).

The overarching goal of preparing for construction is risk reduction. Be sure your preparation activities are reducing risks, not increasing them.
If you want to develop high-quality software, attention to quality must be part of the software-development process from the beginning to the end. Attention to quality at the beginning has a greater influence on product quality than attention at the end.
Part of a programmer’s job is to educate bosses and co-workers about the software-development process, including the importance of adequate preparation before programming begins.
The kind of project you’re working on significantly affects construction prerequisites—many projects should be highly iterative, and some should be more sequential.
If a good problem definition hasn’t been specified, you might be solving the wrong problem during construction.

If good requirements work hasn’t been done, you might have missed important details of the problem. Requirements changes cost 20 to 100 times as much in the stages following construction as they do earlier, so be sure the requirements are right before you start programming.

If a good architectural design hasn’t been done, you might be solving the right problem the wrong way during construction. The cost of architectural changes increases as more code is written for the wrong architecture, so be sure the architecture is right, too.

Understand what approach has been taken to the construction prerequisites on your project, and choose your construction approach accordingly.

# Key Construction Details

''Programming conventions'' - should be detailed for a project to improve consistency and legibility.

''The technology wave'' - where are we? Early/middle/late.

''Programming into the language'' - If a language lacks constructs that you want, invent your own conventions, standards, classes, etc.

Every programming language has strengths and weaknesses. Be aware of the specific strengths and weaknesses of the language you’re using.
Establish programming conventions before you begin programming. It’s nearly impossible to change code to match them later.
More construction practices exist than you can use on any single project. Consciously choose the practices that are best suited to your project.
Ask yourself whether the programming practices you’re using are a response to the programming language you’re using or controlled by it. Remember to program into the language, rather than programming in it.
Your position on the technology wave determines what approaches will be effective - or even possible. Identify where you are on the technology wave, and adjust your plans and expectations accordingly.
 
# Design in Construction

''Challenges'' - design is wicked, design is a sloppy process, design is about trade-offs and priorities. Design is heuristic.

''Managing complexity'' - accidental & essential difficulties. Anyone can only keep so much detail in their head at once. Divide the system into subsystems. Keep routines short.

''Desirable characteristics'' - minimal complexity, ease of maintenance, loose coupling, extensibility, reusability, high fan-in, low fan-out, portability, leanness, stratification, standard techniques.

Different levels of design:

# Software system
# Division into subsystems/packages (Jira: components?)
# Division into classes within packages
# Division into data and functions within classes
# Internal function design