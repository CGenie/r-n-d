---
layout: post
title: How to make asynchronous calls
---

# {{ page.title }}

Basically asynchronous calls can be done in 2 ways: with or without a broker.

_Pros_ of a broker is that our workers can fail/die and we can still pass the failed message to some other worker.

_Cons_ of a broker is that we have an additional point of failure in the architecture.

## With broker

For quite a big list of queueing systems see [http://queues.io/](http://queues.io).

### [Celery project](http://www.celeryproject.org)

[List of stable brokers](http://docs.celeryproject.org/en/latest/getting-started/brokers/index.html):

* RabbitMQ
* Redis
* MongoDB (experimental but has monitoring & remote control)

[Sample code](http://docs.celeryproject.org/en/latest/userguide/tasks.html#basics)

Also has some goodies like:

* [periodic scheduler](http://celery.readthedocs.org/en/latest/userguide/periodic-tasks.html)
* [webhooks](http://celery.readthedocs.org/en/latest/userguide/remote-tasks.html)
* [complex workflows](http://celery.readthedocs.org/en/latest/userguide/canvas.html)
* [monitoring](http://celery.readthedocs.org/en/latest/userguide/monitoring.html)

And it’s [customizable](http://celery.readthedocs.org/en/latest/userguide/extending.html)

Note that the webhook mechanism allows for [blocking and non-blocking calls](https://github.com/celery/celery/tree/master/examples/celery_http_gateway/):

```
$ curl http://localhost:8000/apply/celery.ping/
{"ok": "true", "task_id": "e3a95109-afcd-4e54-a341-16c18fddf64b"}
$ curl http://localhost:8000/e3a95109-afcd-4e54-a341-16c18fddf64b/status/
{"task": {"status": "SUCCESS", "result": "pong", "id": "e3a95109-afcd-4e54-a341-16c18fddf64b"}}
```

### [uWSGI Spooler](https://uwsgi-docs.readthedocs.org/en/latest/Spooler.html)

This is a simple task executor with queue. Tasks are represented as files in some directory so this directory is our “broker”.


### [NATS](http://nats.io)

A simple, high-performance messaging platform implementing publish-subscribe mechanism.
Clustering [is supported](http://nats.io/docs/#gnatsd_configuration).

### [Taskmaster](https://github.com/dcramer/taskmaster)

“_Taskmaster is a simple distributed queue designed for handling large numbers of one-off tasks._”
This is supposed to be a simpler and high-performance alternative to Celery.

### [Redis PUB/SUB mechanism](http://redis.io/topics/pubsub)

[A simple example](https://gist.github.com/jobliz/2596594)

Also some messaging systems are built around it like [Resque](http://resquework.org/),
[RestMQ](http://restmq.com/), [RQ](http://python-rq.org/).

### [Apache Samza](http://samza.apache.org/)

Basically it is a database system with a pub/sub mechanism.
Here’s a [very good explanation](http://blog.confluent.io/2015/03/04/turning-the-database-inside-out-with-apache-samza/)


## Without broker

### [ReactiveX](http://reactivex.io/)

This is an implementation of Observer pattern on steroids.

[Examples of use in Python](https://github.com/ReactiveX/RxPY/tree/master/examples)

[More about reactive programming](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

### [Python 3 Futures](https://docs.python.org/3/library/concurrent.futures.html)

Part of Python 3 standard library ([asyncio](https://docs.python.org/3/library/asyncio.html)).


### [Tornado Webserver](http://www.tornadoweb.org)

[It has some support for concurrency](http://www.tornadoweb.org/en/stable/coroutine.html) (with own Futures mechanism).

