<script setup>
  import GameArena from './components/GameArena.vue';
</script>

<template>
 <div class="h-screen overflow-hidden">
  <div class="absolute top-[42px] left-[68.11px]">
    <img src="/src/assets/TicTacToe.svg" class="w-[42px] h-[58px]" alt="">
  </div>
  <div class="h-full flex justify-center items-center rounded-[46px]">
    <div class="p-[20px] box rounded-[46px]">
      <GameArena class="h-[440px] w-[440px] child" />
    </div>
  </div>
 </div>
</template>

<style scoped>
.box {
border: 2px dashed  rgba(255, 255, 255, 0.5);
border-radius: 46px;
}

</style>