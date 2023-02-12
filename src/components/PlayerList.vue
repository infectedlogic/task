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
.btn{
  border-radius : 25px;
}
.btn-secondary{
  background : rgb(9, 166, 219);
  color :white;
}

</style>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      players: [],
      sortBy: 'Rank',
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
      return filteredPlayers.sort((a, b) => a[this.sortBy] - b[this.sortBy]);
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
      this.players = this.players.sort((a, b) => a[this.sortBy] - b[this.sortBy]);
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

