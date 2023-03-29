<template>
  <MasonryWall :items="items" :ssr-columns="1" :column-width="100" :gap="16">
    <template #default="{ item, index }">
      <div class="card flex items-center justify-center bg-slate-50 text-black rounded-md">
        <img :src="item.file" :alt="`Image ${index}`" />
      </div>
    </template>
  </MasonryWall>
</template>


<script>
import MasonryWall from '@yeger/vue-masonry-wall'

export default {
  components: {
    MasonryWall,
  },
  data() {
    return {
      items: [2,2,2,2,2,2,2,2,2,2,2,2,2],
    }
  },
  mounted() {
    fetch('https://diana.dh.gu.se/api/shfa/image/?format=json&limit=25')
      .then(response => response.json())
      .then(data => {
        // Set the results data to the retrieved JSON data
        this.items = data.results;
      });

  }
}
</script>

<style>

</style>

