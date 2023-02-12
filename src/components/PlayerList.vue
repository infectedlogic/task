<template>
  <div>
    <div class="form-group">
      <label for="sort-by">Sort by:</label>
      <select v-model="sortBy" @change="sortPlayers">
        <option value="Rank">Rank</option>
        <option value="Hits">Hits</option>
      </select>
    </div>
    <div class="form-group">
      <label for="filter-by">Filter by:</label>
      <select v-model="filterBy">
        <option value="Rank">Rank</option>
        <option value="AgeThatYear">Age</option>
        <option value="Hits">Hits</option>
      </select>
      <input type="text" v-model="filterValue" @input="filterPlayers"/>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Id</th>
          <th>Rank</th>
          <th>Player</th>
          <th>Age</th>
          <th>Hits</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="player in filteredPlayers" :key="player.Id">
          <td>{{ player.id }}</td>
          <td v-if="player.Rank !== '' ">{{ player.Rank }}</td>
          <td v-else>None</td>
          <td>{{ player.Player }}</td>
          <td>{{ player.AgeThatYear }}</td>
          <td>{{ player.Hits }}</td>
          <td>
            <button class="btn btn-secondary" @click="showDetail(player)">Details</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style scoped>
  .form-group {
    display: grid;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  .form-group label {
    font-weight: bold;
  }
  
  .form-group select,
  .form-group input[type="text"] {
    width: 200px;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }
  
  table {
    width: 100%;
    margin-top: 20px;
    border-collapse: collapse;
  }
  
  th, td {
    padding: 10px;
    border: 1px solid #ccc;
  }
  
  th {
    background-color: #eee;
  }
  
  button.btn {
    padding: 10px 20px;
    background-color: rgb(7, 135, 239);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      players: [],
      sortBy: 'id',
      filterBy: 'Rank',
      filterValue: ''
    }
  },
  computed: {
    filteredPlayers() {
      let filteredPlayers = this.players;
      if (this.filterValue) {
        filteredPlayers = filteredPlayers.filter(player => {
          return player[this.filterBy].toString().toLowerCase()==(this.filterValue.toLowerCase());
        });
      }
      return filteredPlayers.sort((a, b) => b[this.sortBy] - a[this.sortBy]);
    }
  },
  mounted() {
    axios.get('https://api.sampleapis.com/baseball/hitsSingleSeason')
      .then(response => {
        this.players = response.data;
      })
      .catch(error => {
        console.error(error);
      });
  },
  methods: {
    sortPlayers() {
      this.players = this.players.sort((a, b) => b[this.sortBy] - a[this.sortBy]);
    },
    filterPlayers() {
      this.players = this.players.sort((a, b) => a[this.sortBy] - b[this.sortBy]);
    },
    showDetail(player) {
    this.$emit('show-detail', player);
  }
}
}
</script>

