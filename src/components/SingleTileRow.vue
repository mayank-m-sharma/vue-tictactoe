<template>
  <div class="flex">
      <SingleTile 
        v-for="tile in rowDetail" 
        :key="tile.id" 
        :tileDetail="tile" 
        class="border border-gray-200"
        @updateTile="handleUpdateTile($event)"
      />
  </div>
</template>

<script>
import SingleTile from './SingleTile.vue';
export default {
    name: "SingleTileRow",
    components: {SingleTile},
    props: {
      rowDetail: {
        type: Array,
        default: () => []
      },
      rowId: {
        type: Number,
        default: ""
      }
    },
    methods: {
      handleUpdateTile(tileId) {
          const tileToUpdate = {
            tileId,
            rowId: this.rowId
          }
          this.$emit('updateTile', tileToUpdate);
      }
    }
}
</script>