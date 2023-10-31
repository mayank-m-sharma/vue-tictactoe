<script setup>
import SingleTile from './SingleTile.vue'
import { ref } from 'vue';
import {row1, row2, row3} from '../constants/tile'
const ticTacToeRow1 = ref(row1);
const ticTacToeRow2 = ref(row2);
const ticTacToeRow3 = ref(row3);

function handleAction(tileId) {
    ticTacToeRow1.value.map(tile => {
        if (tile.id === tileId) {
            tile.icon = "/src/assets/circleIcon.svg"
        }
    })
}

</script>

<template>
    <!-- row 1 - -->
    <div class="flex flex-col">
        <div class="flex">
             <SingleTile 
                v-for="tile in ticTacToeRow1"
                class="border w-28 h-28 border-gray-200 cursor-pointer hover:bg-slate-50" 
                :key="tile.id"
                @click="handleAction(tile.id)" 
                :iconSrc="tile.icon" 
            />
        </div>
        <div class="flex">
             <SingleTile 
                v-for="tile in ticTacToeRow2"
                class="border w-28 h-28 border-gray-200 cursor-pointer hover:bg-slate-50" 
                :key="tile.id" 
                :iconSrc="tile.icon" 
            />
        </div>
        <div class="flex">
             <SingleTile 
                v-for="tile in ticTacToeRow3"
                class="border w-28 h-28 border-gray-200 cursor-pointer hover:bg-slate-50" 
                :key="tile.id" 
                :iconSrc="tile.icon" 
            />
        </div>
    </div>
</template>