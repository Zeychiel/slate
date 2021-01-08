---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - liverooms
  - errors
  - models

search: true

code_clipboard: true
---

# Introduction

Welcome to the Gamelive API!

This Documentation describes how to use our API to create awesome liverooms.

General concepts:
A user needs to be registered in order to create a room. He then becomes admin of this room.
The Room model describes the state of a public room, including its customization.
The Liveroom model describes a room being live, i.e. a websocket is on to update the information (questions, timers...) to all the connected users.

A user entering a room "joins" this room and is then connected to the Liveroom.

An example is available is available here: dev.gamelive.gg

This API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to fill in pull requests to update or correct this documentation.

# Authentication

Endpoints having the notice <aside class="notice">Requires auth token</aside> require an auth token to be passed as parameter.

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>
