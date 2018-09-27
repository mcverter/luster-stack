***CELERY***
[Celery Docs](http://docs.celeryproject.org/en/latest/index.html)

- Distributes work across threads or machines.
- A task queue’s input is a unit of work called a task
- Celery communicates via messages, usually using a broker to mediate between clients and workers.


GETTING STARTED
===============
* Create Celery Instance
```python
from celery import Celery

# module_name, message_broker, results_storage
app = Celery('tasks', broker='pyamqp://guest@localhost//', backend='rpc://')

@app.task
def add(x, y):
    return x + y

```
* Run Celery Worker
```
> celery -A ${module_name} worker --loglevel=info
```

* Working With Task
```
# import task
>>> from tasks import add

# use `delay` to start task
>>> result add.delay(4, 4)

# check whether finished (Boolean)
>>> result.ready()

# get result (async_timeout, error_handler)
>>> result.get(timeout=1, propogate=False)


```

* Configure configure configure


Next Steps
==========
* Start Worker

```
 celery -A proj worker -l info

-------------- celery@halcyon.local v4.0 (latentcall)
---- **** -----
--- * ***  * -- [Configuration]
-- * - **** --- . broker:      amqp://guest@localhost:5672//
- ** ---------- . app:         __main__:0x1012d8590
- ** ---------- . concurrency: 8 (processes)
- ** ---------- . events:      OFF (enable -E to monitor this worker)
- ** ----------
- *** --- * --- [Queues]
-- ******* ---- . celery:      exchange:celery(direct) binding:celery
--- ***** -----
```

* Daemonize Worker

```
 celery multi start w1 -A proj -l info
celery multi v4.0.0 (latentcall)
> Starting nodes...
    > w1.halcyon.local: OK

    $ celery multi start 10 -A proj -l info -Q:1-3 images,video -Q:4,5 data \
        -Q default -L:4,5 debug
```

* Run Tasks
>>> add.delay(2, 2)
>>> add.apply_async((2, 2), queue='lopri', countdown=10)
>>> add(2, 2)

* Results
>>> res = add.delay(2, 2)
>>> res.id
>>> res.get(timeout=1, propagate=False)
>>> res.state { or call failed() successful() }

* Signatures to tasks
>>> add.signature((2, 2), countdown=10)
>>> can curry arguments
```
>>> s1 = add.s(2, 8)
is
>>> s2 = add.s(2)
>>> s2 = add.s(2)

```


* Canvas Primitives
-- group
-- chain
-- chord
-- map
-- starmap
-- chunks
