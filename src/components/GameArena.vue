<template>
  <div class="flex flex-col bg-[#253952] rounded-[46px] shadow-lg">
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
            crossImage: '/src/assets/closeIconBig.svg',
            circleImage: '/src/assets/circleIcon.svg',
            crossImageActive: '/src/assets/closeIconYellow.svg',
            currentTurn: TURN_VALUE.PLAYER_1,
            isGameOver: false,
            winner: null,
            movesSoFar: 0
        }
    },
    components: {
        SingleTileRow
    },
    methods: {
        handleUpdateTile(tile) {
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
            this.movesSoFar++;
            if (this.movesSoFar >= 5) {
                const winingStatus = this.checkWiningStatus();
            }
        },
        checkWiningStatus() {
            this.gameDetails.forEach(row => {
                if (this.isMatchingValuesInRow(row)) {
                    console.log(`${this.winner} wins`)
                    this.isGameOver = true;
                    return true;
                }
            })
            for (let col = 0; col <= 2; col ++) {
                if (this.isMatchingValuesInColumn(col)) {
                    console.log(`${this.winner} wins`)
                    this.isGameOver = true;
                    return true;
                }
            }
            if (this.isLeftDigonalHasMatchingValues()) {
                console.log(`${this.winner} wins`)
                this.isGameOver = true;
                return true;
            }
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
                this.crossImage = "/src/assets/closeIconYellow.svg";
            } else {
                this.winner = "PLAYER 2"
            }
            return true;
         },
         isLeftDigonalHasMatchingValues() {
            let cellValue = this.gameDetails[0].rowDetail[0].cellValue;
            let middleCellValue = this.gameDetails[1].rowDetail[1].cellValue;
            let lastCellValue = this.gameDetails[2].rowDetail[2].cellValue; 
            let arr = [cellValue, middleCellValue, lastCellValue]
            const leftDigonalMatchStatus = this.isDigonalHasMatchingValues(arr);
            if (leftDigonalMatchStatus) {
                return true;
            } 
            cellValue = this.gameDetails[0].rowDetail[2].cellValue;
            middleCellValue = this.gameDetails[1].rowDetail[1].cellValue;
            lastCellValue = this.gameDetails[2].rowDetail[0].cellValue; 
            arr = [cellValue, middleCellValue, lastCellValue]
            const rightDigonalMatchStatus = this.isDigonalHasMatchingValues(arr);
            if (rightDigonalMatchStatus) {
                return true;
            } 
         },
         isDigonalHasMatchingValues(arr) {
            if (arr[0] === null || arr[1] === null || arr[2] === null) {
                return false;
            } else if (arr[0] === arr[1] && arr[1] === arr[2]) {
                if (arr[0] === CELL_CONSTANT.CROSS) {
                this.winner = "PLAYER 1"
                } else {
                    this.winner = "PLAYER 2"
                }
                return true;
            } 
        },
        isMatchingValuesInColumn(col) {
            let firstColumnValue = this.gameDetails[0].rowDetail[col].cellValue;
            if (firstColumnValue === null ) {
                return false;
            }
            let row = 0;
            firstColumnValue = this.gameDetails[0].rowDetail[col].cellValue;
                for (row = 1; row <= 2; row++) {
                    if (this.gameDetails[row].rowDetail[col].cellValue === null || firstColumnValue !== this.gameDetails[row].rowDetail[col].cellValue) {
                        return false;
                    }
                else if (row === 2) {
                    console.log('win')
                    if (firstColumnValue === CELL_CONSTANT.CROSS) {
                        this.winner = "PLAYER 1"
                    } else {
                        this.winner = "PLAYER 2"
                    }
                    console.log(`${this.winner} wins`)
                    this.isGameOver = true;
                    return true;
                }
            }
        }
    }
}
</script>