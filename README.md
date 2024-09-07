# powpubsub

`powpubsub` a publish/subscribe (Pub/Sub) signaling server that incorporates a proof-of-work (PoW) mechanism to mitigate abuse. It is designed to facilitate the discoverability and initial signaling in a peer-to-peer network while maintaining minimal server involvement. By requiring a computational effort for client requests, powpubsub reduces the potential for spam and server overload.

## API

```
HTTP GET /info

HTTP GET /publish?channel=channel1&message=hello&salt=abcdef&pow=000000

HTTP GET /subscribe?channel=channel1&channel=channel2&timeout=10&timestamp=1234567890&salt=abcdef&pow=000000
```

The `/info` endpoint returns a JSON object with the current server time the salt used for PoW, and the current difficulty parameters.

```json
{
  "time": 1234567890,
  "salt": "abcdef",
  "difficulty_publish": 6,
  "difficulty_subscribe": 6
}
```

The `/publish` endpoint publishes a message to a channel.

The pow must be a string such that the first `n` bits of the SHA256 hash of the query string are zero, where `n` is a difficulty parameter set by the server.

The `/subscribe` endpoint subscribes to one or more channels and returns any messages that have been published to those channels since the specified timestamp. The response is a JSON object with the messages and the current server time.

```json
{
  "time": 1234567890,
  "messages": [
    {
      "channel": "channel1",
      "message": "hello",
      "timestamp": 1234567890
    }
  ]
}
```
