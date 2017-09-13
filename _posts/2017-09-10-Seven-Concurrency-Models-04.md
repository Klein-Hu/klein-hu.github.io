---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 04
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: false
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "Actor". The sample code is in [Elixir](http://elixir-lang.org/).

**Actor Model** is about many processes (definition of process in Erlang or Elixir is different from normal OS concept) communicate via message queue. It does not need any lock and state sharing between processes.

Obviously, if process A wants to send a reply to process B, the original message and reply message should have the same "tag"/"id" to identify the context.



### Open Readings
1. Hewitt, Meijer and Szyperski: [The Actor Model](https://channel9.msdn.com/Shows/Going+Deep/Hewitt-Meijer-and-Szyperski-The-Actor-Model-everything-you-wanted-to-know-but-were-afraid-to-ask)

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)