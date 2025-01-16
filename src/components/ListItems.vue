<template>
  <v-container class="box-container">
    <v-row>
      <v-col v-for="item in items" :key="item.id" cols="12" md="6" lg="3">
        <CardItem :item="item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
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

onMounted(() => {
  axios.get<Item[]>('https://www.freetestapi.com/api/v1/destinations')
    .then(response => {
      items.value = response.data.slice(0, 12);
      console.log(items);
    })
    .catch(error => {
      console.error('There was an error!', error);
    });
});
</script>

<style scoped>
.box-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

</style>
