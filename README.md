
Twitch IRC client via WebSocket, Before:

```js
var socket = new WebSocket(url);
socket.onopen = function (e) {
  socket.send("CAP REQ :twitch.tv/tags twitch.tv/commands twitch.tv/membership");
  socket.send("PASS SCHMOOPIIE");
  socket.send("NICK " + "justinfan8989");
  socket.send("USER " + "justinfan" + "8989");
  socket.send("JOIN " + "#twitchplayspokemon");
};

var i = 10;
socket.onmessage = function (msg) {
  if (i-- > 0) console.log(msg);
  else socket.close();
}
```

After:

```js
var rxSocket = new RxWebSocket(url);

rxSocket.open().subscribe(function (socket) {
    socket.send("CAP REQ :twitch.tv/tags twitch.tv/commands twitch.tv/membership");
    socket.send("PASS SCHMOOPIIE");
    socket.send("NICK " + "justinfan8989");
    socket.send("USER " + "justinfan" + "8989");
    socket.send("JOIN " + "#twitchplayspokemon");
  });

rxSocket.message().take(10).subscribe(function (msg) {
    console.log(msg.data);
  });
```

or

```js
var rxSocket = new RxWebSocket(url);
rxSocket.open()
  .doOnNext(function (socket) {
    socket.send("CAP REQ :twitch.tv/tags twitch.tv/commands twitch.tv/membership");
    socket.send("PASS SCHMOOPIIE");
    socket.send("NICK " + "justinfan8989");
    socket.send("USER " + "justinfan" + "8989");
    socket.send("JOIN " + "#twitchplayspokemon");
  })
  .flatMap(function (socket) {
    return rxSocket.message();
  })
  .take(10)
  .map(function (msg) { return msg.data })
  .subscribe(function (msg) {
    console.log(msg);
  });
```

