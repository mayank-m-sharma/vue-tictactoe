<script setup>
const {iconSrc} = defineProps(['iconSrc'])
</script>

<template>
    <div>
        <img :src="iconSrc" alt="">
    </div>
</template>