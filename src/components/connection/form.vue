<template>
  <v-form>
    <v-card>
      <v-card-text>
        <v-row>
          <v-col cols="8">
            <v-text-field
              v-model="host"
              label="Host"
              outlined/>
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="port"
              label="Port"
              outlined
              type="number"/>

          </v-col>
        </v-row>
        <v-textarea
          v-model="tx"
          label="Tx packets"
          outlined
          auto-grow/>
      </v-card-text>
      <v-card-actions>
        <v-spacer/>
        <v-btn v-if="!client" @click="connect" color="success">
          connect
        </v-btn>
        <template v-else>
          <v-btn @click="disconnect" color="error">disconnect</v-btn>
          <v-btn @click="send" color="primary">send</v-btn>
        </template>
      </v-card-actions>
    </v-card>
  </v-form>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import net from 'net'

  @Component
export default class ConnectionForm extends Vue {
  host = 'localhost'
  port = 8888
  tx = '24 30 31 32 32 32 38 33 33 36 39 34 2C 41 34 5F 47 53 5F 52 45 54 41 49 4C 2C 31 30 30 30 2C 30 2C 33 36 32 37 38 39 39 31 2C 31 32 36 39 31 32 31 32 35 2C 56 2C 2C 2C 2C 63 0C'
  rx = ''
  client: net.Socket | null = null

  connect () {
    this.client = net.createConnection({ host: this.host, port: this.port }, () => {
      if (!this.client) return
      console.log('connected to server!')
      // this.client.write('world!\r\n')
    })
    this.client.on('data', (data) => {
      if (!this.client) return
      console.log(data.toString())
      this.rx = data.toString()
      this.client.end()
    })
    this.client.on('end', () => {
      this.client = null
      console.log('disconnected from server')
    })
  }

  disconnect () {
    if (!this.client) return
    this.client.destroy()
    this.client = null
  }

  send () {
    const ts = this.tx.split(' ').map(t => parseInt('0x' + t))
    console.log(ts.length)
    const buf = Buffer.from(ts)
    if (!this.client) return
    this.client.write(buf)
  }
}
</script>

<style scoped>

</style>
