# Usage examples

## Establishing connection

To establish connection to the signaling server, clients must use the WebSocket protocol. The connection URL should be in the format described in: [signaling section](../index.md)

##### Socket creation example

```js
const token = "...";
const url =  "wss://core.example.com/signaling/v1?token=${token}&force=false";
const socket = new WebSocket(url);

function handleMessage(event) {
  const data = JSON.parse(event.data);
  console.log(data);
}

socket.addEventListener("message", handleMessage);
socket.connect();
```

Than first message should be a [handshake `state`](../handshake/state.md) message, which is sent by the server after the connection is established. Clients must wait for this message before sending any requests.

Depends on sip registration state, you may receive one complete handshake state like this:

```json
{
  "timestamp": 1754313722205,
  "lines": [null, null],
  "handshake": "state",
  "keepalive_interval": 30000,
  "registration": {
    "status": "registered"
  }
}
```

Or a sequence of handshake `unregistered` state and registration process messages (see /events/session):

```json
{
  "timestamp": 1754313039148,
  "lines": [null, null],
  "handshake": "state",
  "keepalive_interval": 30000,
  "registration": {
    "status": "unregistered"
  }
}
```

```json
{
  "event": "registering"
}
```

```json
{
  "event": "registered",
  "phrase": "ok"
}
```

## Transactions (acknowledgements)

Transactions are used to acknowledge requests sent by the client. Each request should include a `transaction` field, which is a unique identifier for that request. The server will respond with the same `transaction` value to confirm that the request was processed.

##### Transactional request example

```json
{
  "line": 0,
  "request": "some_request",
  "transaction": "transaction-6"
}
```

##### Transactional response example

```json
{
  "line": 0,
  "response": "ack",
  "transaction": "transaction-6"
}
```

##### Reference implementation example

```js
let messagePromises = new Map(); // Stores Promises for message acknowledgments
let transactionCounter = 0;

function handleMessage(event) {
  const data = JSON.parse(event.data);
  if (data.transaction != null && messagePromises.has(data.transaction)) {
    const { resolve } = messagePromises.get(data.transaction);
    resolve(data.payload);
    messagePromises.delete(data.transaction);
  } else {
    // Handle other types of messages
    console.log("Received:", data);
  }
}

function sendMessage(message) {
  return new Promise((resolve, reject) => {
    const transaction = `transaction-${transactionCounter++}`;
    const messageWithId = { ...message, transaction };
    messagePromises.set(transaction, { resolve, reject });
    socket.send(JSON.stringify(messageWithId));
  });
}
```

## Keepalive

The client should send keepalive messages to maintain the connection. The interval for these messages is specified in the `keepalive_interval` field of the handshake state message.

##### Keepalive setup example

```js
const keepaliveInterval = setInterval(function() {
  const response = await sendMessage({ handshake: "keepalive" });
  console.log(response); // { handshake: "keepalive", transaction: "transaction-0" }
}, handshake.keepalive_interval);
```

## Making call

To initiate a call, the client must send a [outgoing call](../requests/call/outgoing_call.md) request with the necessary parameters and establish a WebRTC connection using the `RTCPeerConnection` API.

To begin create media stream according to WebRTC specifications (see [media devices](https://webrtc.org/getting-started/media-devices))

```js
const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
```

Create [PeerConnection](https://webrtc.org/getting-started/peer-connections) and assign the media stream to it:

```js
const pc = new RTCPeerConnection();
stream.getTracks().forEach((track) => pc.addTrack(track, stream));
```

Create an offer using the `createOffer` method of the `RTCPeerConnection` instance this will generate an SDP that will be used in the `outgoing_call` request:

```js
const offer = await pc.createOffer();
```

Prepare ice candidates listeners to handle local ICE candidates that will be generated during the call setup process. and send them to the server using the [ice_trickle](../requests/line/ice_trickle.md) request:

```js
pc.addEventListener("icecandidate", (event) => {
  if (event.candidate) {
    sendMessage({
      request: "ice_trickle",
      line: 0,
      candidate: event.candidate,
    });
  }
});
```

Prepare signaling listeners for further call events, such as `accepted`, `ringing`, `hangup`, etc (see events/call).
As the call is accepted, the server will send an event with the `jsep` field containing the SDP answer, this SDP should be assigned to the remote description of the `RTCPeerConnection` instance for complete WebRTC negotiation.

```js
// ... handleMessage body
switch (data.event) {
  case "calling":
    console.log("Call is being established", data);
    break;
  case "ringing":
    console.log("Call is ringing", data);
    break;
  case "accepted":
    console.log("Call is accepted", data);
    const answer = new RTCSessionDescription(data.jsep);
    await pc.setRemoteDescription(answer);
    break;
  case "hangingup":
    console.log("Call is being hung up", data);
    break;
  case "hangup":
    console.log("Call was ended", data);
    // Clean up resources, close PeerConnection, etc.
    break;
}
```

After that you can send the `outgoing_call` request with the created SDP offer.
Important to notice that setting the local description is happening after sending the `outgoing_call` request. Its because setting the local description will trigger the generation of ICE candidates emmidately, and we want to ensure that the server is ready to receive them.

```js
const call_id = "qwertyuiopasdfghjklzxcvbnm"; // Generate a unique call ID

const response = await sendMessage({
  request: "outgoing_call",
  transaction: "transaction-1",
  line: 0,
  call_id: call_id,
  number: "1234567890",
  jsep: offer,
});

await pc.setLocalDescription(offer);

// console log:
//
// - Call is being established:
//  {
//    "event": "calling",
//    "line": 0,
//    "call_id": "qwertyuiopasdfghjklzxcvbnm"
//  }
//  
// - Call is ringing:
//  {
//    "event": "ringing",
//    "line": 0,
//    "call_id": "qwertyuiopasdfghjklzxcvbnm"
//  }
//  
// - Call is accepted:
//  {
//    "event": "accepted",
//    "line": 0,
//    "call_id": "qwertyuiopasdfghjklzxcvbnm",
//    "jsep": {
//      "type": "answer",
//      "sdp": "..."
//    }
//  }
//
// - Call is established now
```

After call is established, you can send the `hangup` request to end the call. This will trigger the `hangingup` event, and once the server confirms the hangup on sip side, a `hangup` event will be sent back.

```js
await sendMessage({
  request: "hangup",
  line: 0,
  call_id: call_id,
});

// console log:
//
// - Call is being hung up:
//  {
//    "code":200,
//    "line":0,
//    "event":"hangingup",
//    "call_id":"qwertyuiopasdfghjklzxcvbnm"
//  }
//  
// - Call was ended:
//  {
//    "code":200,
//    "line":0,
//    "reason":"to BYE",
//    "event":"hangup",
//    "call_id":"qwertyuiopasdfghjklzxcvbnm"
//  }
//
// - Call is ended now
```

## Receiving a call

To receive a call the client should add listener for [incoming call](../events/call/incoming_call.md) events.

```js
// ... handleMessage body
switch (data.event) {
  // ... other cases
  case "incoming_call":
    console.log("Incoming call received", data);
    handleIncomingCall(data);
    break;
}
```

To accept an incoming call, create a new `RTCPeerConnection` and assign the media stream to it, similar to the outgoing call process.

```js
async function handleIncomingCall(data) {
  const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
  const pc = new RTCPeerConnection();
  stream.getTracks().forEach((track) => pc.addTrack(track, stream));
}
```

Then set up the ICE candidates listener and handle the incoming SDP offer from the `incoming_call` event.

```js
async function handleIncomingCall(data) {
  // ... previous code
  pc.addEventListener("icecandidate", (event) => {
    if (event.candidate) {
      sendMessage({
        request: "ice_trickle",
        line: data.line,
        candidate: event.candidate,
        call_id: data.call_id,
      });
    }
  });
}
```

Next, set the remote description with the incoming SDP offer, create an answer and send it back to the server using the [accept_call](../requests/call/accept.md) request.

```js
async function handleIncomingCall(data) {
  // ... previous code
  await pc.setRemoteDescription(new RTCSessionDescription(data.jsep));
  const answer = await pc.createAnswer();
  await pc.setLocalDescription(answer);
  await sendMessage({
    request: "accept",
    line: data.line,
    call_id: data.call_id,
    jsep: answer,
  });
}

// The call is now accepted and established
```

Now you can hang up the call using the `hangup` request, similar to the outgoing call process.

```js
await sendMessage({
  request: "hangup",
  line: data.line,
  call_id: data.call_id,
});

// Call is ended
```
