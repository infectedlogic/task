<template>
  <div>
    <PlayerList v-if="!selectedPlayer" @showDetail="showPlayerDetail"/>
    <PlayerDetail v-if="selectedPlayer" :player="selectedPlayer" @close="closePlayerDetail"/>
  </div>
</template>
<script>
import PlayerList from './PlayerList.vue'
import PlayerDetail from './PlayerDetail.vue'
export default {
  components: {
    PlayerList,
    PlayerDetail
  },
  data() {
    return {
      selectedPlayer: null
    }
  },
  methods: {
    showPlayerDetail(player) {
      this.selectedPlayer = player
    },
    closePlayerDetail() {
      this.selectedPlayer = null
    }
  }
}
</script>


