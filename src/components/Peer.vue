<template>
  <div>
    <div>
      {{msg}}
    </div>
    <div>
      Peer ID: {{peerId}}
    </div>
    <div>
      Connect to:
      <input v-model="connectTo"/>
      <button @click="connect()">Tengjast</button>
    </div>

    <div>
      Skilaboð:
      <input v-model="skilabod"/>
      <button
        @click="send()">Senda</button>
    </div>
    <hr />
    <div>{{stream}}</div>
  </div>
</template>

<script>
import Peer from 'peerjs'
let connection
let incoming

export default {
  name: 'Peer',
  data () {
    return {
      msg: 'Peer 2 peer',
      peerId: 'fetching ...',
      connectTo: '',
      stream: '',
      skilabod: ''
    }
  },
  created: function () {
    this.peer = new Peer({key: 'puevvvakmqs38fr'})

    const self = this
    this.peer.on('open', function (id) {
      console.log('Peer ID: ' + id)
      self.peerId = id
    })
    this.peer.on('connection', this.incomingConnection)
    this.peer.on('error', this.error)
  },
  methods: {
    error: function (err) {
      console.log(err)
    },
    incomingConnection: function (c) {
      const self = this
      c.on('open', function () {
        console.log('Ný tenging frá ', c.peer)
        incoming = c
      })
      c.on('data', function (mssg) {
        console.log('Got: ' + mssg)
        self.stream += mssg
      })
      c.on('error', this.error)
    },
    connect: function () {
      const self = this
      const id = this.connectTo
      console.log('Reyni að tengjast við ' + id)
      connection = this.peer.connect(id, {
        label: 'chat',
        serialization: 'none',
        metadata: {message: 'spjall?'}
      })
      connection.on('open', function () {
        console.log('Tengdur við ', id)
        connection.on('data', function (mssg) {
          console.log('Inn: ' + mssg)
          self.stream += mssg
        })
      })
      connection.on('error', this.error)
    },
    send: function () {
      const conn = connection || incoming
      if (conn) {
        conn.send(this.skilabod)
        this.stream += this.skilabod + ' '
      }
    }
  }
}
</script>

<style scoped>
</style>
