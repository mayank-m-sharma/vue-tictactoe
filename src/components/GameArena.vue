<template>
  <div class="flex flex-col">
    <p>Current player - {{ currentTurn }}</p>
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

        }
    },
    components: {
        SingleTileRow
    },
    methods: {
        handleUpdateTile(tile) {
            if (this.gameDetails[tile.rowId].rowDetail[tile.tileId].iconSrc !== '') {
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
        }
    }
}
</script>