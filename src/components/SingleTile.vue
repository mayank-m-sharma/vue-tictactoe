<template>
    <div class="border border-dashed border-gray-100 h-28 w-28 cursor-pointer" @click="handleClick(tileDetail.id)">
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
