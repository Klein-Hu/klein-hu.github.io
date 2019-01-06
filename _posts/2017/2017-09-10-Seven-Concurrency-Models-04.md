---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 04
categories: [Reading]
tags: [Design, Design-pattern, Concurrency, Parallel]
fullview: false
comments: true
published: true
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "Actor". The sample code is in [Elixir](http://elixir-lang.org/).

**Actor Model** is about many processes (definition of process in Erlang or Elixir is different from normal OS concept) communicate via message queue. It does not need any lock and state sharing between processes. Each actor has a mailbox that stores messages until they’re handled

Obviously, if process A wants to send a reply to process B, the original message and reply message should have the same "tag"/"id" to identify the context.

A little more complex version of Actor Model is something like below. Any of actors could crash and supervising process will create a new instance of actor process to replace the crashed one. The **link** between supervisor and actor are *bidirection*. An actor also need to have a timeout for actions to make sure there is no deadlock on waiting. 

From above diagram, the **Error-Kernel** is the supervisor process.

![Supervisor-Worker]({{ site.url }}/assets/images/Supervisor-Worker.svg)

There are two ways of constructing a software design: 
  - One way is to make it so simple that there are obviously no deficiencies
  - The other way is to make it so complicated that there are no obvious deficiencies.

A software system's **error kernel** is the part that must be correct if the system is to function correctly. 
  - Defensive programming is an approach to achieving fault tolerance by trying to anticipate possible bugs.
  - Actor programs tend to avoid defensive programming and subscribe to the **“let it crash”** philosophy, allowing an actor’s supervisor to address the problem instead.

Alan Kay:

> The key in making great and growable systems is much more to design how its modules communicate rather than what their internal properties and behaviors should be.

Since actor model is based on "message", actors could be on different machines. All we need here is a reliable mesasging system. From some point of view, the actor is more OO than object.

### Open Readings
1. Hewitt, Meijer and Szyperski: [The Actor Model](https://channel9.msdn.com/Shows/Going+Deep/Hewitt-Meijer-and-Szyperski-The-Actor-Model-everything-you-wanted-to-know-but-were-afraid-to-ask)
2. Tony Hoare: [The Emperor’s Old Clothes](http://zoo.cs.yale.edu/classes/cs422/2011/bib/hoare81emperor.pdf)
3. Joe Armstrong’s Lambda Jam presentation [Systems That Run Forever Self-Heal and Scale](https://www.infoq.com/presentations/self-heal-scalable-system)



---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)