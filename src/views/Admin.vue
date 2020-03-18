<template>
  <div class="admin">
    <h1>Admin</h1>
    <form method="POST" @submit.prevent="createProposal">
      <label for="proposal">Proposal:</label>
      <input type="text" name="proposal" id="proposal" v-model="proposal">
      <button type="submit">Send Proposal</button>
    </form>
    <div>
      <h2>Votes:</h2>
      <div>
        <p>Yes: {{ yesVotes }}</p>
        <p>No: {{ noVotes }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import openSocket from 'socket.io-client'
import api from '../services/api'

export default {
  name: 'Admin',
  data() {
    return {
      socket: {},
      proposal: '',
      yesVotes: 0,
      noVotes: 0,
    }
  },
  methods: {
    createProposal() {
      console.log('create proposal')
      this.socket.emit('new proposal', this.proposal)
    }
  },
  created() {
    this.socket = openSocket(api.defaults.baseURL)
    this.socket.on('yes vote', () => {
      this.yesVotes += 1
    })
    this.socket.on('no vote', () => {
      this.noVotes += 1
    })
  }
}
</script>

<style>

</style>