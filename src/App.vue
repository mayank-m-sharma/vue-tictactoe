<template>
 <div class="h-screen overflow-hidden">
  <div class="absolute top-[42px] left-[68.11px]">
    <img src="/src/assets/TicTacToe.svg" class="w-[42px] h-[58px]" alt="">
  </div>
  <div class="h-full w-full flex justify-center items-center rounded-[46px]">
    <div class="flex justify-around items-center w-full">
      <PlayerInfoTab  
        :isCurrentPlayerTurn="checkIfCurrentPlayerTurn(TURN_VALUE.PLAYER_1)"
        :player-info="getPlayerInfo(TURN_VALUE.PLAYER_1)" 
      />
      <div class="p-[20px] box rounded-[46px]">
        <GameArena class="h-[440px] w-[440px] child" :currentTurn="currentTurn" @updateCurrentTurn="handleUpdateCurrentTurn($event)" />
      </div>
      <PlayerInfoTab 
        :isCurrentPlayerTurn="checkIfCurrentPlayerTurn(TURN_VALUE.PLAYER_2)"
        :player-info="getPlayerInfo(TURN_VALUE.PLAYER_2)" 
      />
    </div>
  </div>
 </div>
</template>

<script>
import GameArena from './components/GameArena.vue';
import PlayerInfoTab from './components/PlayerInfoTab.vue'
import {TURN_VALUE} from './constants/tile';
export default {
  name: "App",
  components: {GameArena, PlayerInfoTab},
  data() {
    return {
      TURN_VALUE,
      currentTurn: TURN_VALUE.PLAYER_1
    }
  },
  methods: {
    handleUpdateCurrentTurn(turn) {
      this.currentTurn = turn
    },
    getPlayerInfo(playerInfo) {
      return {
        playerNumber: "PLAYER 1",
        playerName: "Mayank Sharma",
        playerIcon: "/src/assets/closeIconSmall.svg"
      }
    },
    checkIfCurrentPlayerTurn(playerId) {
      return this.currentTurn === playerId; 
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