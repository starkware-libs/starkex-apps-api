
# Streaming API

The Streaming API is aimed to allow integrating clients to receive all relevant notifications on StarkEx events.

Client subscribe to a stream of events with the operator, and the operator working on top of StarkEx will push to clients the relevant events, specifically about StarkEx transactions.

Technically, the connection is established using the [websockets protocol](https://datatracker.ietf.org/doc/html/rfc6455). It is up to the operator to define specific ports or any other configuration for establishing the connection.

The general sequence is the client sending a subscription request, the operator responding with a connection ID and connecting.
From this point, the operator is to push relevant events on the channel to the requesting subscriber.
The client or operator may choose to terminate the connection with a proper cancelling message.


![General Flow](https://www.plantuml.com/plantuml/png/LSz1geCm4CRn_PnYbkyjTFQb56_WmXECSGe17KDcKkZj6ssBTHVu_VE5TEQSlImp0AHwYY4cLiUXZ1Po72KZ6mudwvDazdMN7c30veRz1UrxvQ_6lDX_bMlJR1HMLGaULyKuGTks6-r2v9dL-8to47KZsf9ZkEWnVLhwUU5Jf5s8JXaHkkGbAhjCjGn1pEvbdfy0q8bEzVil)

## Subscription

The payload of messages is based on JSON RPC messages (same as the syncrhonous API). With the same schemas defined in [the API](./api/nft-apps-openrpc.json).

Specifically, a subscription is done using the `starkex_subscribe` method:
```javascript

{
    "jsonrpc":"2.0",
    "id": 1,
    "method": "starkex_subscribe",
    "params": {
        "event_type" : ["mint"]
    }
}

```
The `event_types` indicate the type of transactions the subscriber would like to receive. Accepted values are defined in the `EVENT_TYPE` enumeration.

The response is simply the connection id (usually a number but can be any unique string):

```javascript
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0x5"
}
```

At this point, the subscriber should start getting notifications indicating that certain events have happened, namely transactions accepted in StarkEx.

In case of any error in establishing a connection, the operator should return the `SUBSCRIPTION_UNSUCCESSFUL` error, with the `data` field quoting the connection id in the request, and any other relevant information.


When a subscriber wishes to stop receiving such notifications, it should send the `starkex_unsubscibe` method with the subscription id received in the result of `starkex_subscribe`. The return value of `unsubscribe` indicates whether the cancellation was sucessful.

## Payload
The schema is of JSON RPC notifications with the relevant transaction objects as defined in the API. In addition, a timestamp must be provided for each event.
The schema is defined in the `STARKEX_EVENT` in [the API](./api/nft-apps-openrpc.json).

Each event includes a timestamp and a status (`PROCESSING_STATUS` - received/processing/accepted/rejected).
The timestamp represents the point in time the relevant status is applicable in StarkEx:
1. `RECEIVED`: when the transaction was received in the StarkEx system.
2. `PROCESSING`: the transaction is batched.
3. `ACCEPTED`: the transaction is proven and the proof is accepted on L1.
4. `REJECTED`: the transaction was rejected in StarkEx, at some point in the flow.

A separate event should be emitted for each combination of transaction and its status. For example, a succesful transaction will effectively have 3 events emitted - 1 for `RECEIVED`, 1 for `PROCESSING` and 1 for `ACCEPTED`.

## Other Considerations

The rate of returned events is not specified, but left to the implementing operator.

Order of events received in the stream of events is not guaranteed to be in order of occurrence.
The `time` property of an event should provide a way to order events in order of occurrence.

Termination of connection is not specified here, but the event producer (StarkEx operator) and the consumer should notify each other of termination so orderly termination can be implementated.

