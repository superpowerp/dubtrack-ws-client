# Dubtrack WS Client

Usage:

```
var socket = new SocketClient({
  host: 'wss://ws.dubtrack.fm',
  secret: 'some-super-secret'
});

function reattach(channel) {
  channel.attach();
  /* Example when using presence on the channel */
  channel.presence.enter();
}

socket.connection.on('connected', () => {
  console.log('channels', socket.channels.all)

  for (let channelName in socket.channels.all) {
    let channel = socket.channels.get(channelName);
    console.log(channel)
    reattach(channel);
  }
  if (interval) {
    clearInterval(interval)
    interval = null
  }


  window.channel = socket.channels.get('room:5845fb26b0b5bf0c00fb03cb')
  window.channel2 = socket.channels.get('test2')
  channel.subscribe(console.log)

  channel.presence.get((err, list) => {
    console.log('list', list)
  })
  channel2.subscribe((msg) => {
    console.log('test2', msg)
  })

  channel.presence.on('enter', console.log)
  channel.presence.on('leave', (msg) => {
    console.log('leave', msg)
  })
})

socket.connection.on('disconnected', () => {
  // interval = setInterval(() => {
  //   socket.connection.connect()
  // }, 1000)
})
socket.connection.on('failed', () => console.log('Failed'))
```
