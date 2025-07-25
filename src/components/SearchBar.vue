<template>
    <div class="search-bar">
      <input
        type="text"
        v-model="city"
        placeholder="Enter city name..."
        @keyup.enter="searchWeather"
        :disabled="loading"
        class="search-input"
        autocomplete="off"
      />
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const city = ref('');
  const emit = defineEmits(['search']);
  const props = defineProps({
    loading: {
      type: Boolean,
      default: false
    }
  });
  
  const searchWeather = () => {
    const trimmedCity = city.value.trim();
    if (trimmedCity && !props.loading) {
      emit('search', trimmedCity);
      city.value = '';
    }
  };
  </script>