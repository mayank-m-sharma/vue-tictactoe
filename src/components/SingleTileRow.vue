<template>
  <div class="flex items-center relative border-b border-dashed border-[#F5F5F5] opacity-[1] last:border-b-0">
      <SingleTile 
        v-for="(tile) in rowDetail" 
        :key="tile.id"
        class="border-l border-[#F5F5F5] border-dashed first:border-l-0"
        :tileDetail="tile" 
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