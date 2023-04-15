<template>
    <v-container class="d-flex align-center justify-center">
      <v-card title="Calculadora de Combustível" class="pa-6">
        <v-text>
            <v-autocomplete
            v-model="startAddress"
            :items="startAddresses"
            label="Endereço de partida"
            item-text="description"
            item-value="place_id"
            placeholder="Digite o endereço de partida"
            @input="getStartAddressSuggestions"
          ></v-autocomplete>
          <v-autocomplete
            v-model="endAddress"
            :items="endAddresses"
            label="Endereço de destino"
            item-text="description"
            item-value="place_id"
            placeholder="Digite o endereço de destino"
            @input="getEndAddressSuggestions"
          ></v-autocomplete>
        </v-text>

        <v-text-field variant="outlined" v-model.number="distance" label="Distância percorrida" suffix="km" class="pt-6"></v-text-field>
        <v-text-field variant="outlined" v-model.number="price" label="Preço do combustível"></v-text-field>
        <v-text-field variant="outlined" v-model.number="consumption" label="Consumo médio" suffix="km/L"></v-text-field>
        <v-text-field variant="outlined" v-model.number="capacity" label="Capacidade do tanque" suffix="Litros"></v-text-field>
        <v-card-actions>
          <v-col>
            <v-text-field variant="outlined" class="py-4" v-model.number="autonomy" label="Autonomia" suffix="km" hint="Distância máxima com o tanque cheio"></v-text-field>
            <v-text-field variant="outlined" v-model.number="fuelCost" label="Custo do combustível" hint="Custo total para a distância informada"></v-text-field>
          </v-col>
        </v-card-actions>
      </v-card>
    </v-container>
  </template>

<script>
import axios from 'axios';
const apiKey = process.env.VUE_APP_API_KEY;

export default {
  name: 'FuelCalculator',
  data() {
    return {
      distance: 100,
      price: 210,
      consumption: 15,
      capacity: 45,
      startAddress: null,
      endAddress: null,
      startAddresses: [],
      endAddresses: [],
      apiKey: apiKey,
    };
  },
  computed: {
    formattedPrice() {
      return this.price.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    },
    autonomy() {
      return this.consumption && this.capacity ? Math.round(this.consumption * this.capacity) : 0;
    },
    fuelCost() {
      let cost = this.distance && this.consumption && this.price ? (this.distance / this.consumption) * this.price : 0;
      return cost.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    },
  },
  methods: {
    async getStartAddressSuggestions(query) {
        console.log("seila", query)
        const response = await axios.get(`https://maps.googleapis.com/maps/api/place/autocomplete/json?input=${query}&types=(cities)&key=${apiKey}`);
            this.startAddresses = response.data.predictions;
        },
        async getEndAddressSuggestions(query) {
        console.log("seila", query)
        const response = await axios.get(`https://maps.googleapis.com/maps/api/place/autocomplete/json?input=${query}&types=(cities)&key=${apiKey}`);
            this.endAddresses = response.data.predictions;
        },
        async calculateDistance() {
            try {
                const response = await axios.get(`https://maps.googleapis.com/maps/api/distancematrix/json?origins=${this.startAddress}&destinations=${this.endAddress}&key=${apiKey}`);
                    const distanceInMeters = response.data.rows[0].elements[0].distance.value;
                    this.distance = distanceInMeters / 1000;
                } catch (error) {
                    console.error(error);
                }
    },
  },
};
</script>
