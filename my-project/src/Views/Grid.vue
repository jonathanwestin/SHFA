<template>
<div v-on:click="closeDropdown">hi
<div class="w-3/4 mx-8">>
  <h1 class="text-5xl m-8">Swedish Rock Art Research Archive</h1>
  <h1 class="text-3xl m-8">SHFA</h1>
    <div class="relative flex items-center">
      <input type="text" id="search" name="search" placeholder="Search Image Database" class="border border-gray-400 py-3 pl-6 pr-10 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400 w-full text-black">
      <div class="absolute inset-y-0 right-0 flex items-center pr-3">
        <svg id="dropdown-toggle" class="w-5 h-5 text-gray-400 cursor-pointer" fill="currentColor" viewBox="0 0 20 20" v-on:click="showDropdown = !showDropdown">
          <path fill-rule="evenodd" d="M13.853 12.146a1 1 0 0 1-1.414 1.414l-1.147-1.147a4.5 4.5 0 1 1 1.414-1.414l1.147 1.147zm3.708 3.708a1 1 0 0 1-1.414 1.414l-3.8-3.8a6 6 0 1 1 1.414-1.414l3.8 3.8zM6.5 10a3.5 3.5 0 1 1 7 0 3.5 3.5 0 0 1-7 0z" clip-rule="evenodd"></path>
        </svg>
        <div id="dropdown" class="absolute z-10 right-0 mt-2 py-2 w-48 bg-white rounded-md shadow-xl" v-if="showDropdown">
          <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 1</a>
          <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 2</a>
          <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 3</a>
        </div>
      </div>
    </div>
  </div>

  <div class="max-w-lg mt-10 mx-6 lg:mx-auto">
    <MasonryWall :items="items" :ssr-columns="1" :column-width="100" :gap="16">
      <template #default="{ item, index }">
        <div
          :style="{ height: `${item}px` }"
          class="card flex items-center justify-center bg-slate-50 text-black rounded-md"
        >
          {{ index }}
        </div>
      </template>
    </MasonryWall>
    <h1 class="font-bold">Restaurants:</h1>
    <ul>
      <li v-for="restaurant in restaurants.data" :key="restaurant.id">
        <h2>{{ restaurant.attributes.name}}</h2>
        <p>{{ restaurant.attributes.description }}</p>
      </li>
    </ul>
    <p></p>
    
  </div>
</div>
</template>

<script lang="ts">
import MasonryWall from '@yeger/vue-masonry-wall'
import { defineComponent } from 'vue'

export default defineComponent({
  components: {
    MasonryWall,
  },
  data() {
    return {
      items: [50, 75, 75, 100, 50, 50, 75, 150, 125, 175, 50, 100, 125],
      restaurants: [],
      showDropdown: false,
    }
  },
  mounted() {
    fetch("http://localhost:1337/api/restaurants")
      .then(response => response.json())
      .then(data => {
        this.restaurants = data;
        console.log(data);
      });
  },
  methods: {
    closeDropdown(event) {
      if (!event.target.closest('#dropdown')) {
        this.showDropdown = false;
      }
    },
  },
})
</script>


<style scoped>
</style>


