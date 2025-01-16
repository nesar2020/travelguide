<template>
  <v-container class="box-container">
    <!-- Continents Tabs -->
    <v-tabs v-model="selectedContinent" color="deep-orange" class="mt-6" fixed-tabs>
      <v-tab v-for="continent in continents" :key="continent" :value="continent" >{{ continent }}</v-tab>
    </v-tabs>

    <!-- List Items Based on Selected Continent -->
    <v-row>
      <v-col v-for="item in filteredItems" :key="item.id" cols="12" md="6" lg="3">
        <CardItem :item="item" />
      </v-col>
      <v-col v-if="filteredItems.length === 0" cols="12" class="text-center mt-12 pa-10 ">
        <v-card class="pa-5 no-results-container" style="min-height: 400px;">
          <v-icon size="50" color="grey lighten-1">mdi-magnify</v-icon>
          <h2 class="mt-4">No results found.</h2>
          <p class="body-1">Try searching again or refining your search criteria.</p>
        </v-card>
      </v-col>
      <v-col v-else v-for="item in filteredItems" :key="item.id" cols="12" md="6" lg="3">
        <CardItem :item="item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import { ref, computed, onMounted,watch } from 'vue';
import axios from 'axios';
import CardItem from '@/components/CardItem.vue';

interface Item {
  id: number;
  name: string;
  country: string;
  continent: string;
  description: string;
  image: string;
  population: string;
  currency: string;
  language: string;
  best_time_to_visit: string;
  top_attractions: string[];
  local_dishes: string[];
  activities: string[];
}
const props = defineProps<{ 
  searchQuery: string;
}>();
const items = ref<Item[]>([]);
const selectedContinent = ref<string>('');
const continents = ref<string[]>([]);


const filteredItems = computed(() => {
  let filtered = items.value;
  let searchText = props.searchQuery.toLowerCase();

  // Filter by selected continent
  if (selectedContinent.value) {
    filtered = filtered.filter(item => item.continent === selectedContinent.value);
  }

  // Filter by search query
  if (searchText) {
    filtered = filtered.filter(item =>
      item.name.toLowerCase().match(new RegExp(searchText, 'i')) ||
      item.country.toLowerCase().match(new RegExp(searchText, 'i')) ||
      item.description.toLowerCase().match(new RegExp(searchText, 'i'))
    );
  }

  return filtered.slice(0, 12);
});

const fetchItems = () => {
  axios.get<Item[]>('https://www.freetestapi.com/api/v1/destinations')
    .then(response => {
      items.value = response.data;
      const uniqueContinents = Array.from(new Set(items.value.map(item => item.continent)));
      continents.value = uniqueContinents;
      //selectedContinent.value = uniqueContinents[0]; 
    })
    .catch(error => {
      console.error('There was an error!', error);
    });
};


onMounted(() => {
  fetchItems();
});
</script>

<style scoped>
.box-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
</style>
