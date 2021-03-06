# node-sonic-channel

[![Build Status](https://img.shields.io/travis/valeriansaliou/node-sonic-channel/master.svg)](https://travis-ci.org/valeriansaliou/node-sonic-channel) [![Test Coverage](https://img.shields.io/coveralls/valeriansaliou/node-sonic-channel/master.svg)](https://coveralls.io/github/valeriansaliou/node-sonic-channel?branch=master) [![NPM](https://img.shields.io/npm/v/sonic-channel.svg)](https://www.npmjs.com/package/sonic-channel) [![Downloads](https://img.shields.io/npm/dt/sonic-channel.svg)](https://www.npmjs.com/package/sonic-channel) [![Buy Me A Coffee](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://www.buymeacoffee.com/valeriansaliou)

**Sonic Channel integration for Node. Used in pair with Sonic, the fast, lightweight and schema-less search backend.**

Sonic Channel lets you manage your Sonic search index, from your NodeJS code. Query your index and get search results, push entries to your index and pop them programmatically.

**🇫🇷 Crafted in Nantes, France.**

## Who uses it?

<table>
<tr>
<td align="center"><a href="https://crisp.chat/"><img src="https://valeriansaliou.github.io/node-sonic-channel/images/crisp.png" height="64" /></a></td>
</tr>
<tr>
<td align="center">Crisp</td>
</tr>
</table>

_👋 You use sonic-channel and you want to be listed there? [Contact me](https://valeriansaliou.name/)._

## How to install?

Include `sonic-channel` in your `package.json` dependencies.

Alternatively, you can run `npm install sonic-channel --save`.

## How to use?

### 1️⃣ Search channel

#### 1. Create the connection

`node-sonic-channel` can be instanciated in search mode as such:

```javascript
var SonicChannelSearch = require("sonic-channel").Search;

var sonicChannelSearch = new SonicChannelSearch({
  host : "::1",            // Or '127.0.0.1' if you are still using IPv4
  port : 1491,             // Default port is '1491'
  auth : "SecretPassword"  // Authentication password (if any)
}).connect({
  connected : function() {
    // Connected handler
    console.info("Sonic Channel succeeded to connect to host (search).");
  },

  disconnected : function() {
    // Disconnected handler
    console.error("Sonic Channel is now disconnected (search).");
  },

  timeout : function() {
    // Timeout handler
    console.error("Sonic Channel connection timed out (search).");
  },

  retrying : function() {
    // Retry handler
    console.error("Trying to reconnect to Sonic Channel (search)...");
  },

  error : function(error) {
    // Failure handler
    console.error("Sonic Channel failed to connect to host (search).", error);
  }
});
```

#### 2. Query the search index

Use the same `sonicChannelSearch` instance to query the search index:

```javascript
// Notice: all methods return 'true' if executed immediately, 'false' if deferred (ie. TCP socket disconnected)

sonicChannelSearch.query("messages", "default", "valerian saliou", function(data, error) {
  // Query results come in the 'data' argument
  // Query errors come in the 'error' argument
});
```

#### 3. Teardown connection

If you need to teardown an ongoing connection to Sonic, use:

```javascript
// Returns: true if proceeding close, false if already closed
sonicChannelSearch.close(function(data, error) {
  // Handle close errors here
});
```

---

### 2️⃣ Ingest channel

#### 1. Create the connection

`node-sonic-channel` can be instanciated in ingest mode as such:

```javascript
var SonicChannelIngest = require("sonic-channel").Ingest;

var sonicChannelIngest = new SonicChannelIngest({
  host : "::1",            // Or '127.0.0.1' if you are still using IPv4
  port : 1491,             // Default port is '1491'
  auth : "SecretPassword"  // Authentication password (if any)
}).connect({
  connected : function() {
    // Connected handler
    console.info("Sonic Channel succeeded to connect to host (ingest).");
  },

  disconnected : function() {
    // Disconnected handler
    console.error("Sonic Channel is now disconnected (ingest).");
  },

  timeout : function() {
    // Timeout handler
    console.error("Sonic Channel connection timed out (ingest).");
  },

  retrying : function() {
    // Retry handler
    console.error("Trying to reconnect to Sonic Channel (ingest)...");
  },

  error : function(error) {
    // Failure handler
    console.error("Sonic Channel failed to connect to host (ingest).", error);
  }
});
```

#### 2. Manage the search index

Use the same `sonicChannelIngest` instance to push text to the search index:

```javascript
// Notice: all methods return 'true' if executed immediately, 'false' if deferred (ie. TCP socket disconnected)

sonicChannelIngest.push("messages", "default", "conversation:1", "I met Valerian Saliou yesterday. Great fun!", function(_, error) {
  // Push errors come in the 'error' argument
});
```

#### 3. Teardown connection

If you need to teardown an ongoing connection to Sonic, use:

```javascript
// Returns: true if proceeding close, false if already closed
sonicChannelIngest.close(function(data, error) {
  // Handle close errors here
});
```

---

### 3️⃣ Control channel

#### 1. Create the connection

`node-sonic-channel` can be instanciated in control mode as such:

```javascript
var SonicChannelControl = require("sonic-channel").Control;

var sonicChannelControl = new SonicChannelControl({
  host : "::1",            // Or '127.0.0.1' if you are still using IPv4
  port : 1491,             // Default port is '1491'
  auth : "SecretPassword"  // Authentication password (if any)
}).connect({
  connected : function() {
    // Connected handler
    console.info("Sonic Channel succeeded to connect to host (control).");
  },

  disconnected : function() {
    // Disconnected handler
    console.error("Sonic Channel is now disconnected (control).");
  },

  timeout : function() {
    // Timeout handler
    console.error("Sonic Channel connection timed out (control).");
  },

  retrying : function() {
    // Retry handler
    console.error("Trying to reconnect to Sonic Channel (control)...");
  },

  error : function(error) {
    // Failure handler
    console.error("Sonic Channel failed to connect to host (control).", error);
  }
});
```

#### 2. Administrate your Sonic server

_You may use the same `sonicChannelControl` instance to administrate your Sonic server._

#### 3. Teardown connection

If you need to teardown an ongoing connection to Sonic, use:

```javascript
// Returns: true if proceeding close, false if already closed
sonicChannelControl.close(function(data, error) {
  // Handle close errors here
});
```

## List of channel methods

### Search channel

* `sonicChannelSearch.query(collection_id<string>, bucket_id<string>, terms_text<string>, done_cb<function>, [options{limit<number>, offset<number>}<object>]?)` ➡️ `done_cb(results<object>, error<object>)`
* `sonicChannelSearch.suggest(collection_id<string>, bucket_id<string>, word_text<string>, done_cb<function>, [options{limit<number>}<object>]?)` ➡️ `done_cb(results<object>, error<object>)`

### Ingest channel

* `sonicChannelIngest.push(collection_id<string>, bucket_id<string>, object_id<string>, text<string>, done_cb<function>)` ➡️ `done_cb(_, error<object>)`
* `sonicChannelIngest.pop(collection_id<string>, bucket_id<string>, object_id<string>, text<string>, done_cb<function>)` ➡️ `done_cb(count<number>, error<object>)`
* `sonicChannelIngest.count<number>(collection_id<string>, [bucket_id<string>]?, [object_id<string>]?, done_cb<function>)` ➡️ `done_cb(count<number>, error<object>)`
* `sonicChannelIngest.flushc(collection_id<string>, done_cb<function>)` ➡️ `done_cb(count<number>, error<object>)`
* `sonicChannelIngest.flushb(collection_id<string>, bucket_id<string>, done_cb<function>)` ➡️ `done_cb(count<number>, error<object>)`
* `sonicChannelIngest.flusho(collection_id<string>, bucket_id<string>, object_id<string>, done_cb<function>)` ➡️ `done_cb(count<number>, error<object>)`

### Control channel

* `sonicChannelControl.trigger(action<string>)` ➡️ `done_cb(_, error<object>)`

## What is Sonic?

ℹ️ **Wondering what Sonic is?** Check out **[valeriansaliou/sonic](https://github.com/valeriansaliou/sonic)**.

## How is it linked to Sonic?

`node-sonic-channel` maintains persistent TCP connections to the Sonic network interfaces that are listening on your running Sonic instance. In case `node-sonic-channel` gets disconnected from Sonic, it will retry to connect once the connection is established again.

You can configure the connection details of your Sonic instance when initializing `node-sonic-channel` from your code; via the Sonic host and port.
