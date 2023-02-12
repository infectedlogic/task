<template>
    <div v-if="player">
      <h3>{{ player.Player }}</h3>
      <table class="table">
        <tbody>
          <tr>
            <td>ID</td>
            <td>{{ player.id }}</td>
        </tr>
          <tr>
            <td>Rank</td>
            <td v-if="player.Rank !== ''">{{ player.Rank }}</td>
            <td v-else>None</td>
          </tr>
          <tr>
            <td>Age</td>
            <td>{{ player.AgeThatYear }}</td>
          </tr>
          <tr>
            <td>Hits</td>
            <td>{{ player.Hits }}</td>
          </tr>
          <tr>
            <td>Year</td>
            <td>{{ player.Year }}</td>
          </tr>
          <tr>
            <td>Bats</td>
            <td>{{ player.Bats }}</td>
          </tr>
        </tbody>
      </table>
      <button class="btn btn-secondary" @click="close">Close</button>
    </div>
  </template>
  
  <style scoped>
  .table td {
    padding: 0.5rem;
    vertical-align: top;
  }

  .btn {
    padding: 10px 20px;
    background-color: rgb(7, 135, 239);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  </style>

  <script>
  export default {
    props: {
      player: Object
    },
    methods: {
      close() {
        this.$emit('close');
      }
    }
  }
  </script>
  