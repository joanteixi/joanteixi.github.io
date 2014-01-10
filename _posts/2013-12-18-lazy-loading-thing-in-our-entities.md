---
layout: post
title: Lazy loading things in our entities with Doctrine events
cover: lazy-loading.jpg
date:   2013-12-19 00:10:00
categories: posts
---

## How lazy load things into our entities using the on load event.

In <a href="http://www.onfan.com">onfan.com</a> we use Redis and mysql; in Redis we store among other things, differents types of counters. When an object is created (object user p.ex) we want to access to his counters that we store in Redis. One way could be create a class that manage objects of this type (user) and call this as a service to inject the values of Redis into this object, so Twig (or whatever) could get the counters calling some "getCounters" or "getLikes" mehtod.

Other way more interesting is add a listener that is called on 'onLoad' event (from Doctrine) and populate some property with the counter using a Clousure, so the function will be called only when it was necessary... let's see an example.

Imagine a classic user class with lots or properties...

{% gist joanteixi/8014610 %}

Every time that user object is created,  Doctrine dispatch onLoad event. We can listen to this event  with a listener that has injected the Redis service.

{% gist joanteixi/58e6c280d117773f2126 %}

So, every time we get a Venue entity and call getLikes() method, the REDIS values are called and injected into our Venue instance. 

    $venue = $em->getRepository('CoreBundle:Venue')->find(1);
    $venue->getLikes() //out counter is injected. 


That's all! Hope will help


