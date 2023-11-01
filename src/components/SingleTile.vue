<template>
    <div 
        class="h-[146px] w-[146px] cursor-pointer flex items-center justify-center" 
        @click="handleClick(tileDetail.id)
    ">
        <img :src="tileDetail.iconSrc" alt="">
    </div>
</template>
<script>
export default {
    name: "SingleTile",
    props: {
        tileDetail: Object,
        default: () => {}
    },
    methods: {
        handleClick(id) {
            this.$emit('updateTile', id);
        }
    }
}
</script>