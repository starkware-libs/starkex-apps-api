
# Streaming API

The Streaming API is aimed to allow integrating clients to receive all relevant notifications on StarkEx events.

Client subscribe to a stream of events with the operator, and the operator working on top of StarkEx will push to clients the relevant events, specifically about StarkEx transactions.

Technically, the connection is established using the [websockets protocol](https://datatracker.ietf.org/doc/html/rfc6455). It is up to the operator to define specific ports or any other configuration for establishing the connection.

The general sequence is the client sending a subscription request, the operator responding with a connection ID and connecting.
From this point, the operator is to push relevant events on the channel to the requesting subscriber.  
The client or operator may choose to terminate the connection with a proper cancelling message.


![General Flow](https://www.plantuml.com/plantuml/png/LSz1geCm4CRn_PnYbkyjTFQb56_WmXECSGe17KDcKkZj6ssBTHVu_VE5TEQSlImp0AHwYY4cLiUXZ1Po72KZ6mudwvDazdMN7c30veRz1UrxvQ_6lDX_bMlJR1HMLGaULyKuGTks6-r2v9dL-8to47KZsf9ZkEWnVLhwUU5Jf5s8JXaHkkGbAhjCjGn1pEvbdfy0q8bEzVil)

