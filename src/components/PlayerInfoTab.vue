<template>
  <div class="flex flex-col space-y-[25px] items-center">
    <p :class="getTabInfoClass">
        {{ getTabInfoText }}
    </p>
    <div class="w-[123px] h-full bg-[#253952] rounded-3xl flex flex-col items-center space-y-2 px-6 py-5"  :class="{'border-[3px] border-[#FB9E01]' : playerInfo.hasWon}">
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
</template>

<script>
export default {
    name: "PlayerInfoTab",
    props: {
        playerInfo: {
            type: Object,
            default: () => {}
        },
        isCurrentPlayerTurn: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        getTabInfoText() {
            return this.playerInfo.hasWon ? 'WINNER' : "Your Turn";
        },
        getTabInfoClass() {
            let infoClass = "text-[#FB9E01] text-[21px] font-medium";
            if (!this.isCurrentPlayerTurn && !this.playerInfo.hasWon) {
                infoClass += ` opacity-0`
            }
            return infoClass;
        }
    }
}
</script>