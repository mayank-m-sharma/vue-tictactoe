<template>
  <div class="flex flex-col">
    <p>Current player - {{ currentTurn }}</p>
    <p>player 1 - x</p>
    <p>player 2 - o</p>
     <SingleTileRow 
        v-for="detail in gameDetails" 
        :key="detail.rowId" 
        :rowId="detail.rowId"
        @updateTile="handleUpdateTile($event)"
        :rowDetail="detail.rowDetail" 
    />
  </div>
</template>

<script>
import {gameDetails, CELL_CONSTANT, TURN_VALUE} from '../constants/tile';
import SingleTileRow from './SingleTileRow.vue';
export default {
    name: "GameArena",
    data() {
        return {
            gameDetails,
            crossImage: '/src/assets/crossIcon.svg',
            circleImage: '/src/assets/circleIcon.svg',
            currentTurn: TURN_VALUE.PLAYER_1,
            isGameOver: false,
            winner: null
        }
    },
    components: {
        SingleTileRow
    },
    methods: {
        handleUpdateTile(tile) {
            console.log(tile)
            if (this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc !== '' || this.isGameOver) {
                return;
            }
            if (this.currentTurn === TURN_VALUE.PLAYER_1) {
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc = this.crossImage;
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].cellValue = CELL_CONSTANT.CROSS;
                this.currentTurn = TURN_VALUE.PLAYER_2
            } else {
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc = this.circleImage;
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].cellValue = CELL_CONSTANT.CIRCLE;
                this.currentTurn = TURN_VALUE.PLAYER_1
            }
            const winingStatus = this.checkWiningStatus();
        },
        checkWiningStatus() {
            this.gameDetails.forEach(row => {
                if (this.isMatchingValuesInRow(row)) {
                    console.log(`${this.winner} wins`)
                    this.isGameOver = true;
                    return true;
                }
            })
            // for (let row = 0; row <= 2; row++) {
            //     if (this.isMatchingValuesInColumn(row)) {
            //         console.log(`${this.winner} wins`)
            //         this.isGameOver = true;
            //         return true;
            //     }
            // }
            // if (this.isLeftDigonalHasMatchingValues() || this.isRightDigonalHasMatchingValues()) {
            //     return true;
            // }
            return false;
        },
        isMatchingValuesInRow({rowDetail}) {
            const cellValue = rowDetail[0].cellValue;
            if (cellValue !== null) {
                for (let row = 1; row <=2; row++) {
                    if (rowDetail[row].cellValue !== cellValue) {
                        return false
                    }
                }
            } else {
                return false;
            }
            if (cellValue === CELL_CONSTANT.CROSS) {
                this.winner = "PLAYER 1"
            } else {
                this.winner = "PLAYER 2"
            }
            return true;
         },
    }
}
</script>