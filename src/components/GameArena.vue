<template>
  <div class="primary-wrapper">
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
import cloneDeep from 'lodash/cloneDeep'
import {initialBoard, CELL_CONSTANT, TURN_VALUE, PLAYERS} from '../constants/tile';
import SingleTileRow from './SingleTileRow.vue';
import crossClickSound from '../assets/effect_tick.mp3'
import circleClickSound from '../assets/click_sound.mp3'
import winSound from '../assets/next_level.mp3'
import drawSound from '../assets/draw-sound.mp3'
export default {
    name: "GameArena",
    data() {
        return {
            gameDetails: cloneDeep(initialBoard),
            crossImage: '/closeIconBig.svg',
            circleImage: '/circleIcon.svg',
            crossImageActive: '/closeIconYellow.svg',
            circleImageActive: '/circleIconBigYellow.svg',
            isGameOver: false,
            winner: null,
            movesSoFar: 0
        }
    },
    props: {
        currentTurn: {
            type: Number,
            default: TURN_VALUE.PLAYER_1
        }
    },
    components: {
        SingleTileRow
    },
    computed: {
        getWiningIcon() {
            return this.winner === PLAYERS.PLAYER_1 ? this.crossImageActive : this.circleImageActive;
        }
    },
    methods: {
        handleUpdateTile(tile) {
            if (this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc !== '' || this.isGameOver) {
                return;
            }
            if (this.currentTurn === TURN_VALUE.PLAYER_1) {
                new Audio(crossClickSound).play()
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc = this.crossImage;
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].cellValue = CELL_CONSTANT.CROSS;
                this.$emit('updateCurrentTurn', TURN_VALUE.PLAYER_2)
            } else {
                new Audio(circleClickSound).play()
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc = this.circleImage;
                this.gameDetails[tile.rowId].rowDetail[tile.tileId].cellValue = CELL_CONSTANT.CIRCLE;
                this.$emit('updateCurrentTurn', TURN_VALUE.PLAYER_1)
            }
            this.movesSoFar++;
            if (this.movesSoFar >= 5) {
                const winingStatus = this.checkWiningStatus();
                if (winingStatus) {
                    const matchDetails = {
                        result: "WIN",
                        winner: this.winner,
                    }
                    const audio = new Audio(winSound);
                    audio.volume = 0.069
                    audio.play();
                    this.resetGame();
                    this.$emit('updateMatchDetails', matchDetails)
                }
            }
            if (this.movesSoFar === 9 && !this.isGameOver) {
                const matchDetails = {
                        result: "DRAW",
                    }
                new Audio(drawSound).play();    
                this.resetGame();
                this.$emit('updateMatchDetails', matchDetails)
            }
        },
        checkWiningStatus() {
            let winingStatus = false;
            this.gameDetails.forEach(row => {
                if (this.isMatchingValuesInRow(row)) {
                    this.isGameOver = true;
                    this.markWiningRow(row);
                    winingStatus = true;
                    return true;
                }
            })
            for (let col = 0; col <= 2; col ++) {
                if (this.isMatchingValuesInColumn(col)) {
                    this.isGameOver = true;
                    this.markWiningColumn(col);
                    winingStatus = true;
                    return true;
                }
            }
            if (this.isDigonalHasMatchingValues()) {
                this.isGameOver = true;
                winingStatus = true;
                return true;
            }
            return winingStatus;
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
                this.winner = PLAYERS.PLAYER_1
            } else {
                this.winner = PLAYERS.PLAYER_2
            }
            return true;
         },
         isDigonalHasMatchingValues() {
            let digonal = this.getDigonalCells('left');
            const leftDigonalMatchStatus = this.checkDigonal(digonal);
            if (leftDigonalMatchStatus) {
                this.markWiningDigonal('left');
                return true;
            } 
            digonal = this.getDigonalCells('right');
            const rightDigonalMatchStatus = this.checkDigonal(digonal);
            if (rightDigonalMatchStatus) {
                this.markWiningDigonal('right');
                return true;
            } 
         },
         checkDigonal(arr) {
            if (arr[0] === null || arr[1] === null || arr[2] === null) {
                return false;
            } else if (arr[0] === arr[1] && arr[1] === arr[2]) {
                if (arr[0] === CELL_CONSTANT.CROSS) {
                    this.winner = PLAYERS.PLAYER_1;
                } else {
                     this.winner = PLAYERS.PLAYER_2;
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
                    if (firstColumnValue === CELL_CONSTANT.CROSS) {
                        this.winner = PLAYERS.PLAYER_1
                    } else {
                        this.winner = PLAYERS.PLAYER_2
                    }
                    return true;
                }
            }
        },
        markWiningRow(row) {
            row.rowDetail.forEach(cell => {
                cell.iconSrc = this.getWiningIcon;
            })
        },
        markWiningColumn(col) {;
            for (let row = 0; row <= 2; row++) {
                this.gameDetails[row].rowDetail[col].iconSrc = this.getWiningIcon;
            }
        },
        markWiningDigonal(direction) {
            if (direction === 'left') {
                this.gameDetails[0].rowDetail[0].iconSrc = this.getWiningIcon;
                this.gameDetails[1].rowDetail[1].iconSrc = this.getWiningIcon;
                this.gameDetails[2].rowDetail[2].iconSrc = this.getWiningIcon;
            } else {
                this.gameDetails[0].rowDetail[2].iconSrc = this.getWiningIcon;
                this.gameDetails[1].rowDetail[1].iconSrc = this.getWiningIcon;
                this.gameDetails[2].rowDetail[0].iconSrc = this.getWiningIcon;
            }
        },
        getDigonalCells(direction) {
            const digonal = [];
            if (direction === 'left') {
                digonal.push(this.gameDetails[0].rowDetail[0].cellValue);
                digonal.push(this.gameDetails[1].rowDetail[1].cellValue);
                digonal.push(this.gameDetails[2].rowDetail[2].cellValue);
            } else {
                digonal.push(this.gameDetails[0].rowDetail[2].cellValue);
                digonal.push(this.gameDetails[1].rowDetail[1].cellValue);
                digonal.push(this.gameDetails[2].rowDetail[0].cellValue);
            }
            return digonal;
        },
        resetGame() {
            setTimeout(() => {
                this.gameDetails = cloneDeep(initialBoard);
                this.isGameOver = false;
                this.winner = null;
                this.movesSoFar = 0;
            }, 3300)
        }
    }
}
</script>