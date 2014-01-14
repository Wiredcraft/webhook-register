# Webhook register

Simple Python webapp that register webhooks.

# Architecture

- **API**: Python web service, using [Falcon](http://falconframework.org/) framework.
- **Storage**: Redis, using [redis-py](https://github.com/andymccurdy/redis-py) as python redis library.

# Example

User registers a webhook:
 - POST request to http://localhost:8000/webhook with userid=testuser
  - Receives asd6f8ads6gf8dsg87s (uuid) as response

User triggers webhook:
 - GET request to http://localhost:8000/webhook/asd6f8ads6gf8dsg87s
  - Triggers a webhook handler with `testuser` as argument.

# Workflow

## Create

1. User send POST request to API with arguments
2. API receive requests, generate unique UUID (or simple hash) for the request
3. API stores in Redis a JSON object that uses the generated hash as key, and store the user request's arguments as the value
4. API returns JSON message to the user:
  - **SUCCESS**: returns code `200`, including the hash as the hook ID; 
  - **ERROR**: returns code `500`, including error message

## Trigger hook

1. User sends POST request to API, including webhook key in the URL
2. API queries Redis and retrieve value associated to the provided webhook key
3. API print on the console the content of the redis value previously fetched
4. API returns JSON message to the user:
  - **SUCCESS**: `201` + "web hook triggered"
  - **ERROR**: `404` + webhook ID not found
  - **ERROR**: `500` + error message

# TODO

- Prepare simple falcon HTTP service
- register POST / GET routes - URL to be defined, for ex.:
  - POST `http://localhost:8000/webhook` - create hook
  - POST `http://localhost:8000/webhook/{id}` - trigger hook ID
- define proper format of the arguments
- establish connection to Redis to get / set / list the keys
- create logic to generate the key ID (hash / UUID)
- Execute a dummy-function that is triggered when webhook is triggered giving userid to the handler.

# Guidelines

- Fully documented code
- Documentation if required to explain complex logic
- Documentation on how to run
- Config file to define (at least): HTTP listening address/port, Redis host/port
- Ensure the messages are saved and loaded as JSON in Redis

# Optional

- Use setup.py to allow installation via pip
