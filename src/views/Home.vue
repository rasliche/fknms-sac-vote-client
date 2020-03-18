<template>
  <div class="home">
    {{ proposal }}
    <button @click="yesVote">Yes</button>
    <button @click="noVote">No</button>
  </div>
</template>

<script>
// @ is an alias to /src
import openSocket from 'socket.io-client'
import api from '../services/api'

export default {
  name: 'Home',
  data() {
    return {
      socket: {},
      rnum: 0,
      proposal: ''
    }
  },
  created() {
    this.socket = openSocket(api.defaults.baseURL)
    this.socket.on('new proposal', proposal => {
      this.proposal = proposal
      console.log('new proposal received to vote on')
    })
  },
  methods: {
    yesVote() {
      console.log('clicked yes')
      this.socket.emit('yes vote')
    },
    noVote() {
      console.log('clicked no')
      this.socket.emit('no vote')
    },
  }
}
</script>
