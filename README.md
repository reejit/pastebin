# Pastebin

A simple implementation of a Pastebin-type service. If you're unfamiliar with what Pastebin is,
see [pastebin.com](https://pastebin.com/).

This is mostly done for learning and fun. It isn't production ready!

# Architecture Diagram

![](arch_diagram.png)

# Building

You can build everything using [Docker](https://www.docker.com/).

To build the Pastebin image:

```bash
# Inside top level directory
docker build -t pastebin-dev -f Dockerfile .
```

To build the database image:

```bash
# Inside the db/ directory
docker build -t pastebin-pg -f Dockerfile .
```

Or, if you'd rather use `docker-compose`:

```bash
# Inside top level directory
docker-compose up --build
```

For local development, you can do:

```bash
# Ensure python 3
pyenv local 3.6.3 # or whichever version you like (3.6+ only)
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

# Tests

Run unit tests via:

```bash
python setup.py test
```

# API

The API is intentionally kept very simple.

## `GET /api/v1/pastes/<shortlink>`

Get the contents of a paste from the given shortlink. If the paste doesn't exist,
a 404 Not Found status code is returned. If the paste exists, it will be returned
in the following JSON body:

```javascript
{
  "paste_content": "hello world! this is my first paste and i'm loving it!"
}
```

## `POST /api/v1/pastes`

Create a new paste. The request body must be JSON, with the following form:

```javascript
{
  "paste_content": "string",
  "time_to_live": 12345, // optional, time to live of the paste in seconds
}
```

If a time to live isn't given the paste will never expire. Otherwise, the paste
will be set to expired - and will no longer be accessible - after the specified time
has passed.

# Still To Do

* Implement paste expiry using Redis queues (DONE)
* Unit tests of async behavior
* Integration tests of system
* Add docstrings to API, classes, and functions
