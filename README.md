# redis-server

[![NPM version](https://badge.fury.io/js/redis-server.svg)](http://badge.fury.io/js/redis-server)
[![Build Status](https://travis-ci.org/BrandonZacharie/node-redis-server.svg?branch=master)](https://travis-ci.org/BrandonZacharie/node-redis-server)
[![Coverage Status](https://coveralls.io/repos/github/BrandonZacharie/node-redis-server/badge.svg?branch=master)](https://coveralls.io/github/BrandonZacharie/node-redis-server?branch=master)

Start and stop a local Redis server in Node.js like a boss.

## Installation

```npm install redis-server```

## Usage

The constructor exported by this module optionally accepts a single argument;
a number or string that is a port or an object for configuration.

### Basic Example
```
const RedisServer = require('redis-server');

// Simply pass the port that you want a Redis server to listen on.
const server = new RedisServer(6379);

server.open((err) => {
  if (err === null) {
    // You may now connect a client to the server bound to port 6379.
  }
});
```

### Configuration

| Property | Type   | Default        | Description
|:---------|:-------|:---------------|:-----------
| port     | Number | 6379           | A port to bind a server to.
| bin      | String | redis-server   | A path to a Redis server binary.
| conf     | String |                | A path to a Redis server configuration file.

A Redis server binary must be available. If you do not have one in $PATH,
provide a path in configuration.

```
const RedisServer = require('redis-server');
const server = new RedisServer({
  port: 6379,
  bin: '/opt/local/bin/redis-server'
});
```

You may use a Redis configuration file instead of configuration object properties that are flags (i.e. `port`). If `conf` is provided, no flags will be passed to the binary.

```
const RedisServer = require('redis-server');
const server = new RedisServer({
  conf: '/path/to/redis.conf'
});
```

### Methods

#### RedisServer#open(callback)

Attempt to open a Redis server. If `callback` is provided, it receives an
`Error` instance if a problem is detected and a `Boolean` is returned; `true`,
iff a process is spawned.

#### RedisServer#close(callback)

Close a Redis server. If `callback` is provided, it receives an `Error` instance
if a problem is detected and a `Boolean` is returned; `true`, iff a process is
killed.

### Properties

#### RedisServer#isOpening

Determine if the instance is starting a Redis server; `true` while a
process is spawning until a Redis server starts or errs.

#### RedisServer#isRunning

Determine if the instance is running a Redis server; `true` once a process
has spawned and the contained Redis server is ready to service requests.

#### RedisServer#isClosing

Determine if the instance is closing a Redis server; `true` while a
process is being killed until the contained Redis server closes.

### Events

#### stdout

Emitted when a Redis server prints to stdout.

#### open

Emitted when a Redis server becomes ready to service requests.

#### close

Emitted when a Redis server closes.

## TODO

- Add Promises
- Support "--slaveof" flag
