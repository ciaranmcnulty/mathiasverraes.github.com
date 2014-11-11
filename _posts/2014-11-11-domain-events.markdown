---
title: Domain Events Braindump
slug: domain-events-braindump
date: 2014-11-11
layout: post
published: true
tags: [blog]
abstract: "Domain Events allow you to segregate the models of different systems"
---


## Boundaries & Messages

A system's design is defined by it's boundaries, and by the way it communicates with other systems. This outward facing design is, all things considered, more important than the internals. It is important whether we are talking about small systems (such as an object) or large systems.

 The communication between systems happens with messages. We can say that our system is bounded when it does not need to know anything about the internal structure of systems it communicates with, and those systems do no need to know anything about our system. When the boundaries are chosen well, we can achieve greater decoupling between systems. This allows us to change the internals of the systems, or replace them altogether.

In software, different systems agree on a protocol that describes the shape of the message. Without it, the message is meaningless. The protocol might be implicit, but typically, the more users a protocol has, the more it needs to be formally described. For example, look at the RFCs that describe the Internet. A protocol is not just an encoding, like JSON or XML. It also describes what properties you'll find in the message, and how to interpret them. Messages can be broadcast, queued, logged, persisted, discarded, ... Depending on the infrastructure, they can arrive late, out of order, more than once, or never at all. If we need high precision, we can't even trust their timestamp. Understanding the infrastructure and taking the constraints into account, is critical here.

## Separate Models

The receiving end does not need to understand anything about the sender, as long as it understands the message. In other words, the model that the sender and the receiver must share, is constrained to the message. The receiver might conform to the model of the sender, but it is often better for the receiver to go its separate way. Having different models for different systems, is very powerful. A grand, unified model for all our systems can be convenient in the beginning, but becomes a burden as the systems grow. In separated models, each individual system is liberated from having to know a lot of information, or from having structures that are not suitable for that system's purpose. Contrast this to typical system integrations using an API that transmits entire entities' states, but leaves it to the client to figure out what has changed and why. In that scenario, the client knows more about the structure of the server, but has less valuable information. Introducing (or discovering) good boundaries, and the communication between them, is the idea behind Context Mapping and Bounded Contexts in Domain-Driven Design.

## Immutability

Messages are immutable. Say A sends a message X to B and C. If B were to change the message before passing it on to D, that would not affect A and C. If you draw on a newspaper you've received, only your own copy is affected. Modeling messages as immutable reflects that idea. A better interpretation is that B reacts to X by sending a message Y to D. Even though X and Y may be mostly identical, it is in fact a new message.

## Envelopes

Sometimes we want to add metadata to a message. These are values that are not part of the actual message, but rather of the surrounding infrastructure: the originating machine, the destination, timestamps, IP's, the browser, user information, ... I like the analogy of an envelope here. You can write on it, without affecting the message. The message is wrapped before being sent, and unwrapped at its arrival. All other systems that are involved in the transport, queuing, or persistence of messages, are unaware of the contents of the envelope. This reflects how postal services have worked for centuries. (Censorship and redaction of messages can be seen as a violation of this contract between sender and receiver.)

## Events

An Event is a type of message that describes something that has happened in the past. This is very useful, because using Events, systems can tell other interested parties something about themselves. The receiver can now react to the event. This is modeled after the way things work in the real world.

Domain Events are a special type of Event messages. A Domain Event is something that has happened in the past, that is of interest to the business. This last distinction means we separate all technical concerns from the domain. Usually, the distinction is very clear. In other cases, we should validate with the business whether they are in fact interested that this Domain Event has happened. In rare cases, the line between technical and domain events is blurred.[^1]

By its nature, an Event has already happened by the time we know about it. We might cheat, and notify other systems of Events before they happen, but this is risky: if we crash before we can achieve our goal, other systems will believe the Event has happened, where in fact it never took place. For a higher level of security, we will persist the Event before we tell anybody about it.

## Event Naming

Precisely because we observe Domain Events from the outside after they have happened, we should name them in the past tense. We'll want to stay as close to the language of the domain as possible. This reduces all kinds of translation errors when talking to the business. I recommend using natural language and make small sentences, such as "Stock was depleted", instead of newspaper-style shortcuts such as "Depleted stock". In my experience, this greatly improves the communication and understanding.


## Event Contents

A Domain Event can be as little as just a name. More often, it will contain values and identifiers to answer questions about who did what to whom, when, why, where, and how much. The values represent the state at the time the Event happened. Ideally, the Event only contains values that are directly relevant to that Event. A receiver that needs more information, should listen to other relevant Events, and keep its own state, in order to make decisions later. The receiver shouldn't query the current state of the sender. Remember that the Event message is received some time after it has happened. The state of the sender might already be different from the state it had at the time it emitted the Event.

## Fat Events

In some cases, we can reduce the burden on the receiver to keep track of older state. We add redundant information to the Domain Event, such as (parts of) the previous state of an Entity. The receiver now knows the old value as well as the changed value. A Domain Event can also contain other values, that have not changed, but that are still useful to the receiver. A typical example are labels that accompany an identity (such as a customer's name, when strictly the Event should only contain the customer's id). We call these "Fat Events". There is some risk involved: by ignoring certain types of Events, and getting all its information from redundant values in the Fat Events, it might miss some of the changes. In our example, the receiver might be working with an older version of the customer's name, because it only listens to sales events and not to events that change the customer's name.

## Representation

It is a mistake to design Domain Events as objects first, and then worry about serializing them. It should be the other way around. Domain Events are first and foremost about communication with other systems (or with yourself in the future, when you'll want to understand what happened in the past -- business intelligence comes to mind). Because of this, we'll want to design them keeping our non-functional requirements in mind: efficiency (binary) versus readability (json), using namespacing to prevent clashes, nested or flat, consumability, ... Here's a simple example:

```
{ "eventName": "acme.my_app.StockWasDepleted",
    "payload": {
        "productId": "2028A516-04C0-4D42-81F4-98A42E2B08BC",
        "depletedOn": "2014-11-11",
        "pricePerUnit": { "amount": 500, "currency": "EUR"}
    }
}

The price could be expressed as "EUR 500" instead of a nested structure. The point here is to choose based on what is best for all involved systems, not just what is easiest to produce or consume in the system you are currently working on.

## Internal representation

The different systems producing and consuming Events can each have their own representation of these events. They can share the code when it's practical, but that's not a necessity. They should pick representations that are idiomatic to the environment: tuples and records in Erlang, objects in Java/C#/PHP, ... We use objects because they are easier to work with than plain strings. The type of the object represents the type of the Event. We make them immutable, to reflect that messages are immutable. The public getters on the objects should return Value Objects: they encapsulate calculations that you might want to do on the values, and they express the domain model better than primitives can.


[^1]: If your business is online advertising, then a click or even an impression on a website is a Domain Event. After all, that's what your business is selling.