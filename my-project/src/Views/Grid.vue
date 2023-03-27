<template>
<div>
  <h1 class="text-5xl m-8">Swedish Rock Art Research Archive</h1>
  <h1 class="text-3xl m-8">SHFA</h1>
 <div class="w-1/2 m-8 mb-32 flex">
  <div class="relative w-full">
    <input type="text" id="search" name="search" placeholder="Search Image Database" class="border border-gray-400 py-3 pl-6 pr-10 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400 w-full text-black drop-shadow-lg">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 absolute cursor-pointer top-4 right-4 text-gray-400" fill="currentColor" viewBox="0 0 24 24" @click="showDropDown" stroke="currentColor">
      <path fill-rule="evenodd" d="M13.853 12.146a1 1 0 0 1-1.414 1.414l-1.147-1.147a4.5 4.5 0 1 1 1.414-1.414l1.147 1.147zm3.708 3.708a1 1 0 0 1-1.414 1.414l-3.8-3.8a6 6 0 1 1 1.414-1.414l3.8 3.8zM6.5 10a3.5 3.5 0 1 1 7 0 3.5 3.5 0 0 1-7 0z" clip-rule="evenodd"></path>
    </svg>
  </div>
  <div id="dropdown" class="absolute z-10 top-12 right-[275px] py-2 w-1/4 bg-white rounded-md shadow-xl" v-if="showDropdown">
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 1</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 2</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 3</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 4</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 5</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 6</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 7</a>
    <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-500 hover:text-white">Option 8</a>
  </div>
</div>

  <!-- Start of Container -->
  <div class="split-container">
  <div class="flex h-screen w-screen">
  <!-- Panel 1 -->
  <div  id="split-0" class="flex-grow bg-gray-200 transition-all duration-500"
       :class="{ 'w-1/3': showThreePanels, 'w-1/2': !showThreePanels }">
      <Map />
  </div>
  <!-- Panel 2 -->
  <div id="split-1" class="flex-grow bg-gray-400 transition-all duration-500 overflow-auto" 
       :class="{ 'w-1/3': showThreePanels, 'w-1/2': !showThreePanels }"
       @click="toggleThreePanels">
 <!--  <div class="relative">
  <button class="absolute top-0 right-0 m-2 p-2 bg-blue-500 text-white rounded-lg">
  </button> -->
  <div class="max-w-lg mt-10 mx-6 lg:mx-auto">
  <div class="p-6">
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
  </div>
 <!--  <ul> Fetch for API
    <li v-for="restaurant in restaurants.data" :key="restaurant.id">
      <h2>{{ restaurant.attributes.name}}</h2>
      <p>{{ restaurant.attributes.description }}</p>
    </li>
  </ul> -->
</div>
</div>
<!-- Panel 3 -->
<transition name="slide">
<div id="split-2" class="flex-grow bg-gray-600"
      :class="{ 'w-1/3': showThreePanels, 'w-0': !showThreePanels }" v-show="showThreePanels">
    <button @click="toggleThreePanels" class="btn btn-circle m-2">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
    </button>
</div>
</transition>


</div>
</div>
<!-- End of Container -->

</div>
</template>

<script lang="ts">
import MasonryWall from '@yeger/vue-masonry-wall'
import Map from '../components/Map.vue'
import { defineComponent } from 'vue'
import Split from 'split.js';

export default defineComponent({
  components: {
    MasonryWall, Map
  },
  data() {
    return {
      items: [50, 75, 75, 100, 50, 50, 75, 150, 125, 175, 50, 100, 125, 50, 75, 75, 100, 50, 50, 75, 150, 125, 175, 50, 100, 125],
      restaurants: [],
      showDropdown: false,
      showThreePanels: false,
    }
  },
  mounted() {
    Split(['#split-0', '#split-1', '#split-2'], {
    minSize: 50,
    dragInterval: 1,
    gutterSize: 10,
    gutterAlign: 'start',
  })
    fetch("http://localhost:1337/api/restaurants")
      .then(response => response.json())
      .then(data => {
        this.restaurants = data;
      });
    document.addEventListener('click', this.closeDropdown);
  },
  methods: {
    toggleThreePanels() 
    {
      this.showThreePanels = !this.showThreePanels;
    },
    showDropDown()
    {
      this.showDropdown = !this.showDropdown;
    },
},
})
</script>


<style scoped>
.slide-enter-active,
.slide-leave-active {
  transition: transform 200ms ease-in;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(100%);
}

.slide-enter-to,
.slide-leave-from {
  transform: translateX(0);
}
</style>


