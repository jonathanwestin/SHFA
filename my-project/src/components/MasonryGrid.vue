<template>
<MasonryWall :items="items" :ssr-columns="1" :column-width="100" :gap="16" :key="raaId">
  <template #default="{ item, index }">
    <div class="card flex items-center justify-center bg-slate-50 text-black rounded-md">
      <img :src="item" :alt="`Image ${index}`" />
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
      type: String, // Change this to String since the raaId is a string
      required: false,
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
          // Update the items array with the image URLs
          this.items = data.results.map(image => image.file);
        })
        .catch(error => {
          console.error('Error fetching images:', error);
        });
    }
  },
},
watch: {
  raaId() {
    this.fetchData();
  },
},
}
</script>

<style>

</style>

