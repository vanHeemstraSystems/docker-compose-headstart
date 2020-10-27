docker-compose-headstart
# Docker Compose - Headstart

Based on "Get started with Docker Compose" at https://docs.docker.com/compose/gettingstarted/

On this page you build a simple Python web application running on Docker Compose. The application uses the Flask framework and maintains a hit counter in Redis. While the sample uses Python, the concepts demonstrated here should be understandable even if you’re not familiar with it.

## Prerequisites

Make sure you have already installed both [Docker Engine](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/). You don’t need to install Python or Redis, as both are provided by Docker images.

## Step 1: Setup

Define the application dependencies.

1. Create a directory for the project:

```
$ mkdir composetest
$ cd composetest
```

2. Create a file called ``app.py`` in your project directory and paste this in:

```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

In this example, redis is the hostname of the redis container on the application’s network. We use the default port for Redis, 6379.

***Handling transient errors***

Note the way the get_hit_count function is written. This basic retry loop lets us attempt our request multiple times if the redis service is not available. This is useful at startup while the application comes online, but also makes our application more resilient if the Redis service needs to be restarted anytime during the app’s lifetime. In a cluster, this also helps handling momentary connection drops between nodes.

3. Create another file called requirements.txt in your project directory and paste this in:

```
flask
redis
```

## Step 2: Create a Dockerfile





## Step 3: Define services in a Compose file




## Step 4: Build and run your app with Compose





## Step 5: Edit the Compose file to add a bind mount




## Step 6: Re-build and run the app with Compose




## Step 7: Update the application




## Step 8: Experiment with some other commands






## Where to go next
