<template>
  <div class="primary-wrapper">
     <div class="flex flex-col space-y-[30px] px-[121px] h-full my-auto items-center justify-center">
        <p class="text-[#F0F0F0] text-[34px]">WINNER</p>
        <PlayerInfoTab :player-info="playerDetail" :showWiningStats="false" />
     </div>
  </div>
</template>

<script>
import audioClip from '../assets/mario_flag_win.mp3'
import PlayerInfoTab from './PlayerInfoTab.vue'
export default {
    name: "DisplayWinner",
    components: {
        PlayerInfoTab
    },
    props: {
        playerDetail: {
            type: Object,
            default: () => {}
        }
    },
    mounted() {
        const audio = new Audio(audioClip);
        audio.volume = 0.12;
        audio.play();
    }
}
</script>

<style>

</style>