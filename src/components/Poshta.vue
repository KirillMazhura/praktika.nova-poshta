<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
    const cities = ref([]);
    const city = ref('');
    const warehouses = ref([]);
    const warehouse = ref('');
    const apiKey = ref("1b171a670b277301b88a30bf59541189");

    const selectWarehouses = () => {
      axios.post("https://api.novaposhta.ua/v2.0/json/", {
        "apiKey": apiKey.value,
        "modelName": "Address",
        "calledMethod": "getWarehouses",
        "methodProperties": {
          "CityName": city.value
        }
      }).then(response => {
        warehouses.value = response.data.data;
      });
    };

    onMounted(() => {
      axios.post("https://api.novaposhta.ua/v2.0/json/", {
        "apiKey": apiKey.value,
        "modelName": "Address",
        "calledMethod": "getCities",
        "methodProperties": {}
      }).then(response => {
        cities.value = response.data.data;
      });
    });
</script>

<template>
    <div class="block">
      <p class="text">Населений пункт:</p>
      <select v-model="city" @change="selectWarehouses" class="select">
        <option value="" disabled selected>Населений пункт</option>
        <option v-for="c in cities" :key="c.CityID">{{ c.Description }}</option>
      </select>
    </div>
    <div class="block">
      <p class="text">Поштове відділення:</p>
      <select v-model="warehouse" class="select">
        <option disabled selected value="">Відділення</option>
        <option v-for="w in warehouses" :key="w.Ref">{{ w.Description }}</option>
      </select>
    </div>
</template>

<style scoped>

</style>
