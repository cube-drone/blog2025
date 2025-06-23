+++
title = 'The Lifecycle of Forgotten Documentation'
date = 2022-11-21T12:00:00-07:00
draft = false
categories = ["software"]
tags = ["documentation", "wiki"]
+++

{{<imgwebp src="no-wiki.png">}}

<!--more-->

The life-cycle of documentation initiatives at every company I've ever worked at has gone like this:

* a new employee, confused by the codebase, goes looking for the codebase's documentation
* they find some old, unmaintained documents from a previous employee, which turn out not to be terribly helpful because most of their assumptions are badly dated, incredibly old, and only describe a tiny fraction of the system
* they make a big project out of writing a new set of docs
* they end up asking a bunch of questions in the chat which helps them understand and document the system
* after a month or two they are pulled on to a big new project, leaving a new set of documents describing only a fraction of the system
* nobody updates the new set of docs
* a new employee, confused by the codebase, goes looking for the codebase's documentation

**But then why do more public and open-source projects have such comparatively excellent documentation?**

> ![](./desire.png)
>
> Desire paths are _paths that form where they're needed_.

Open-source projects must on-board people at a significantly higher rate than internal projects.

The team building software at your company? Pretty slow growing. You're probably onboarding someone, what, once a year? Twice a year?

Comparatively, products like React or Godot are probably onboarding six to ten new people every _hour_.

Once someone has been asked the same question multiple times in the same day, a FAQ appears, then docs.

It’s also different if you’re trying to actively _court_ developers: if you want people to use your APIs without having met you, those really need documentation, examples, a first class on-boarding experience. But… most internal teams have a much more informal onboarding experience.

Things also change pretty frequently on internal projects, and there’s always someone around who knows the answer to any given question you might have. If you do develop documentation, it’s usually for people outside of your team who consume your team’s product, not for internal development. "Everyone Online All The Time" is a really bad environment for the development of a comprehensive MANUAL D’THING. In fact, I’d go so far as to say that that manual probably isn’t necessary and it is silly to keep trying to write it.

## But What About Cross-Team Interop?

This opinion is _controversial_, but...

I've had a long standing policy of just delivering a test suite demonstrating the operation of new {{<sidenote endpoints.>}} (editor's note from the future:) and, as of recently, as our company has grown, some OpenAPI spec. {{</sidenote>}}

It's not pretty, but it's sufficient. Software developers can understand code-as-documentation, I believe, especially when the code is designed to show off and probe a specific endpoint to demonstrate exactly what it's intended to do - and this documentation is by its very nature guaranteed to be correct and stay correct. We provide and verify our contract with client using our mountains of test suites.

This is a little loosey-goosey; if behavior isn't tested it's not guaranteed - but if your opposite number probes you for answers about behavior that’s undefined in the tests, that’s also behavior that there aren’t any tests for. Write some tests to clarify and test that!

Divining the use of an endpoint entirely through constructed examples really, really isn't as nice or professional as Real Documentation, but it’s… often good enough!
