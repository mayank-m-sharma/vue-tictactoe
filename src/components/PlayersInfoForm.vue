<template>
  <div class="primary-wrapper"> 
    <form @submit.prevent="handleFormSubmit()" class="flex flex-col space-y-[34px] items-center h-full w-full px-[70px] py-[55px]">
        <h1 class="text-[#F0F0F0] text-[23px]">
            Welcome to <span class="text-[#FBA202]">TIC TAC TOE</span>
        </h1>
        <div class="flex flex-col justify-center space-y-2 w-full">
            <p class="text-xs text-white">PLAYER 1</p>
            <input v-model="player1Name" class="h-12 w-full bg-transparent border border-[#F0F0F0] rounded-[2px] text-white px-3 py-5" type="text" placeholder="Player 1">
        </div>
        <div class="flex flex-col justify-center space-y-2 w-full">
            <p class="text-xs text-white">PLAYER 2</p>
            <input v-model="player2Name" class="h-12 w-full bg-transparent border border-[#F0F0F0] rounded-[2px] text-white px-3 py-5" type="text" placeholder="Player 1">
        </div>
        <button class="btn-class">
            Continue
        </button>
    </form>
  </div>
</template>

<script>
export default {
    name: "PlayersInfoForm",
    data() {
        return {
            player1Name: "",
            player2Name: "",
        }
    },
    methods: {
        handleFormSubmit() {
            if (this.player1Name === "" || this.player2Name === "") return;
            const playersList = [this.player1Name, this.player2Name];
            this.$emit('updatePlayersList', playersList);
        }
    }
}
</script>

<style scoped>
.btn-class {
    width: 300px;
    color: white;
    height: 49px;
    background: transparent linear-gradient(270deg, #FBA202 0%, #FF5800 100%) 0% 0% no-repeat padding-box;
    box-shadow: 0px 3px 6px #000000;
    border-radius: 2px;
}
</style>