<template>
  <v-container class="box-container">
    <v-row>
      <v-col v-for="item in items" :key="item.id" cols="12" md="6" lg="3">
        <v-card
          class="mx-auto mt-12 atrcard"
          max-width="374"
        >
          <template v-slot:loader="{ isActive }">
            <v-progress-linear
              :active="isActive"
              color="deep-purple"
              height="4"
              indeterminate
            ></v-progress-linear>
          </template>

          <v-img height="250" :src="item.image" cover></v-img>

          <v-card-item>
            <v-card-title>{{ item.name }}</v-card-title>

            <v-card-subtitle>
              <span class="me-1">Visit time: {{ item.best_time_to_visit }}</span>

              <v-icon
                color="error"
                icon="mdi-fire-circle"
                size="small"
              ></v-icon>
            </v-card-subtitle>
          </v-card-item>

          <v-card-text>
            <v-row align="center" class="mx-0">
              <v-rating
                :model-value="makeStars(item.population)"
                color="amber"
                density="compact"
                size="small"
                half-increments
                readonly
              ></v-rating>

              <div class="text-grey ms-4">{{ makeStars(item.population) }}  ({{ makeReviews(item.population) }})</div>
            </v-row>

            <div class="my-4 text-subtitle-1">{{ item.country }}</div>

            <div>{{ item.activities.join(', ') }}</div>
          </v-card-text>


        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';

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

  const makeStars = (population: string): string => {
    return (parseFloat(population.substring(0, 4))%5).toFixed(1); 
  };
  const makeReviews = (population: string): number => {
    return Math.ceil(parseFloat(population.substring(0, 4))*5); 
  };
</script>


<style scoped>
.box-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
.atrcard{
  min-height: 460px;
}
</style>
