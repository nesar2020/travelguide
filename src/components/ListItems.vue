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
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import { ref, computed, onMounted, watch } from 'vue';
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

const items = ref<Item[]>([]);
const selectedContinent = ref<string>('');
const continents = ref<string[]>([]);

const filteredItems = computed(() => {
  if (!selectedContinent.value) {
    return items.value;
  }
  const allItems: Item[] = items.value.filter(item => item.continent === selectedContinent.value);
  return [...allItems.values()].slice(0, 12);
});

const fetchItems = () => {
  axios.get<Item[]>('https://www.freetestapi.com/api/v1/destinations')
    .then(response => {
      items.value = response.data;
      const uniqueContinents = Array.from(new Set(items.value.map(item => item.continent)));
      continents.value = uniqueContinents;
      selectedContinent.value = uniqueContinents[0]; // Default to the first continent
    })
    .catch(error => {
      console.error('There was an error!', error);
    });
};

//watch(selectedContinent, (newContinent) => {});

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
