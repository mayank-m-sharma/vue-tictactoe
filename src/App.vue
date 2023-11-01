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
        :hasMatchDrew="matchStatus === 'DRAW'"
        :player-info="players[0]" 
      />
      <div class="p-5 box rounded-[46px]">
        <GameArena 
          v-if="players.length" 
          class="h-[440px] w-[440px] child" 
          :currentTurn="currentTurn" 
          @updateCurrentTurn="handleUpdateCurrentTurn($event)" 
          @updateMatchDetails="handleUpdateMatchDetails($event)"
        />
        <PlayersInfoForm v-if="!players.length" class="h-[440px] w-[440px]" @updatePlayersList="updatePlayersList($event)" />
      </div>
      <PlayerInfoTab 
        v-if="players.length"
        :isCurrentPlayerTurn="checkIfCurrentPlayerTurn(TURN_VALUE.PLAYER_2)"
        :hasMatchDrew="matchStatus === 'DRAW'"
        :player-info="players[1]" 
      />
    </div>
  </div>
 </div>
</template>

<script>
import GameArena from './components/GameArena.vue';
import PlayerInfoTab from './components/PlayerInfoTab.vue'
import {TURN_VALUE, MATCH_STATUS, PLAYERS} from './constants/tile';
import PlayersInfoForm from './components/PlayersInfoForm.vue';
export default {
  name: "App",
  components: {GameArena, PlayerInfoTab, PlayersInfoForm},
  data() {
    return {
      TURN_VALUE,
      currentTurn: TURN_VALUE.PLAYER_1,
      players: [],
      matchStatus: MATCH_STATUS.ONGOING,
      winner: ""
    }
  },
  methods: {
    handleUpdateCurrentTurn(turn) {
      this.currentTurn = turn
    },
    checkIfCurrentPlayerTurn(playerId) {
        switch(this.matchStatus) {
          case MATCH_STATUS.ONGOING: 
            return this.currentTurn === playerId;
          case MATCH_STATUS.WIN: 
            return this.currentTurn === TURN_VALUE[this.winner];
          case MATCH_STATUS.DRAW: 
            return true;
        }
    },
    updatePlayersList([player_1, player_2]) {
        const player_1_details = {
          playerName: player_1,
          playerNumber: "PLAYER 1",
          playerIcon: "/src/assets/closeIconSmall.svg",
          score: 0,
          hasWon: false
        }
        const player_2_details = {
          playerName: player_2,
          playerNumber: "PLAYER 2",
          playerIcon: "/src/assets/circleIconSmall.svg",
          score: 0,
          hasWon: false
        }
        this.players = [player_1_details, player_2_details]
    },
    handleUpdateMatchDetails(matchDetails) {
      if (matchDetails.result === MATCH_STATUS.WIN) {
        this.matchStatus = MATCH_STATUS.WIN;
        switch(matchDetails.winner) {
          case PLAYERS.PLAYER_1: 
            this.players[0].score++;
            this.players[0].hasWon = true;
            this.winner = PLAYERS.PLAYER_1;
            break;
          case PLAYERS.PLAYER_2:
            this.players[1].score++;
            this.players[1].hasWon = true;
            this.winner = PLAYERS.PLAYER_2;
            break;
        }
      } else if (matchDetails.result === MATCH_STATUS.DRAW) {
          this.matchStatus = MATCH_STATUS.DRAW;
      }
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