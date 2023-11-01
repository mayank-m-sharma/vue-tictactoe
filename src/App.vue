<template>
 <div class="h-screen overflow-hidden">
  <div class="absolute top-[42px] left-[68.11px]">
    <img src="/src/assets/TicTacToe.svg" class="w-[42px] h-[58px]" alt="">
  </div>
  <div class="h-full w-full flex justify-center items-center rounded-[46px]">
    <div class="flex justify-around items-center w-full">
      <PlayerInfoTab  
        v-if="players.length"
        :isCurrentPlayerTurn="checkIfCurrentPlayerTurn(TURN_VALUE.PLAYER_1)"
        :player-info="players[0]" 
      />
      <div class="p-5 box rounded-[46px]">
        <GameArena v-if="players.length" class="h-[440px] w-[440px] child" :currentTurn="currentTurn" @updateCurrentTurn="handleUpdateCurrentTurn($event)" />
        <PlayersInfoForm v-if="!players.length" class="h-[440px] w-[440px]" @updatePlayersList="updatePlayersList($event)" />
      </div>
      <PlayerInfoTab 
        v-if="players.length"
        :isCurrentPlayerTurn="checkIfCurrentPlayerTurn(TURN_VALUE.PLAYER_2)"
        :player-info="players[1]" 
      />
    </div>
  </div>
 </div>
</template>

<script>
import GameArena from './components/GameArena.vue';
import PlayerInfoTab from './components/PlayerInfoTab.vue'
import {TURN_VALUE} from './constants/tile';
import PlayersInfoForm from './components/PlayersInfoForm.vue';
export default {
  name: "App",
  components: {GameArena, PlayerInfoTab, PlayersInfoForm},
  data() {
    return {
      TURN_VALUE,
      currentTurn: TURN_VALUE.PLAYER_1,
      players: []
    }
  },
  methods: {
    handleUpdateCurrentTurn(turn) {
      this.currentTurn = turn
    },
    checkIfCurrentPlayerTurn(playerId) {
      return this.currentTurn === playerId; 
    },
    updatePlayersList([player_1, player_2]) {
        const player_1_details = {
          playerName: player_1,
          playerNumber: "PLAYER 1",
          playerIcon: "/src/assets/closeIconSmall.svg"
        }
        const player_2_details = {
          playerName: player_2,
          playerNumber: "PLAYER 2",
          playerIcon: "/src/assets/circleIconSmall.svg"
        }
        this.players = [player_1_details, player_2_details]
    }
  }
}
</script>

<style scoped>
.box {
border: 2px dashed  rgba(255, 255, 255, 0.5);
border-radius: 46px;
}

</style>