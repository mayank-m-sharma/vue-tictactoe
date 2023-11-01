<template>
  <div class="flex flex-col space-y-[15px] items-center" :class="{' animate__animated animate__tada' : playerInfo.hasWon}">
    <div class="flex flex-col space-y-[25px] items-center">
        <p v-if="showWiningStats" :class="getTabInfoClass">
            {{ getTabInfoText }}
        </p>
        <div class="w-[123px] h-full bg-[#253952] rounded-3xl flex flex-col items-center space-y-2 px-6 py-5" :class="getHighlightBorderClass">
            <div class="flex flex-col space-y-3 items-center justify-center">
                <p class="text-[#FB9E01] text-[13px]">
                    {{ playerInfo.playerNumber }}
                </p>
                <p class="text-white text-center line-clamp-1 text-[18px] font-medium">
                    {{ playerInfo.playerName }}
                </p>
            </div>
            <img class="h-[43px] w-[43px]" :src="playerInfo.playerIcon" alt="">
        </div>
    </div>
    <div v-if="showWiningStats" class="flex space-x-3 items-center">
        <div 
            v-for="(score, index) in playerInfo.score"
            :key="index"
            class="h-2.5 w-2.5 rounded-full bg-white" 
        />
        <div 
            v-for="(match, index) in remainingMatches"
            :key="index"
            class="h-2.5 w-2.5 rounded-full bg-white opacity-[0.05]" 
        />
    </div>
  </div>
</template>

<script>
export default {
    name: "PlayerInfoTab",
    data() {
        return {
            totalRounds: 6
        }
    },
    props: {
        playerInfo: {
            type: Object,
            default: () => {}
        },
        isCurrentPlayerTurn: {
            type: Boolean,
            default: false
        },
        hasMatchDrew: {
            type: Boolean,
            default: false
        },
        showWiningStats: {
            type: Boolean,
            default: true
        },
    },
    computed: {
        getTabInfoText() {
            if (this.playerInfo.hasWon) {
                return "WINNER";
            }
            if (this.hasMatchDrew) {
                return "DRAW"
            }
            return "Your Turn";
        },
        getTabInfoClass() {
            let infoClass = "text-[#FB9E01] text-[21px] font-medium";
            if (!this.isCurrentPlayerTurn && !this.playerInfo.hasWon && !this.hasMatchDrew) {
                infoClass += ` opacity-0`
            }
            return infoClass;
        },
        getHighlightBorderClass() {
            if (this.playerInfo.hasWon || this.hasMatchDrew) {
                return 'border-[3px] border-[#FB9E01]'
            }
        },
        remainingMatches() {
            return this.totalRounds - this.playerInfo.score;
        }
    }
}
</script>