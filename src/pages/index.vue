<template>
  <Header @search="filterData" /> 
  <ListItems :searchQuery="searchQuery" />
  
</template>


<script lang="ts" setup>
import { ref } from 'vue';

  const searchQuery = ref<string>(''); 
  const filterData = (query: string) => { 
    searchQuery.value = query;
  };
</script>

