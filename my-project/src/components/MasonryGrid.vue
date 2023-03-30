<template>
  <MasonryWall :items="items" :ssr-columns="1" :column-width="100" :gap="16">
    <template #default="{ item, index }">
      <div class="card flex items-center justify-center bg-slate-50 text-black rounded-md">
        <img :src="item.file" :alt="`Image ${index}`" />
      </div>
      {{raaId}}
    </template>
   
  </MasonryWall>
</template>


<script>
import MasonryWall from '@yeger/vue-masonry-wall'

export default {
  components: {
    MasonryWall,
  },
  props: {
    raaId: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      items: [],
    }
  },
  mounted() {
  },
  methods: {
  fetchData() {
    if (this.raaId) {
      fetch(`https://diana.dh.gu.se/api/shfa/image/?format=json&raa_id=${this.raaId}`)
        .then(response => response.json())
        .then(data => {
          // Filter the results data to only include images with the matching raa_id
          this.items = data.results.filter(image => image.raa_id === this.raaId);
        });
    }
  },
  },
  watch: {
      raaId: {
        handler() {
          this.fetchData();
        },
        immediate: true,
      },
    },
  }
</script>

<style>

</style>

