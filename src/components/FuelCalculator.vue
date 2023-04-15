<template>
    <v-container class="d-flex align-center justify-center">
        <v-card width="400">
            <v-card-title ali>Calculadora de Combustível</v-card-title>
            <v-card-text>
                <v-col>
                    <v-text-field v-model="distance" label="Distância percorrida (km)"></v-text-field>
                </v-col>
                <v-col>
                    <v-text-field v-model="price" label="Preço do combustível (R$)"></v-text-field>
                </v-col>
                <v-col>
                    <v-text-field v-model="consumption" label="Consumo médio (km/L)"></v-text-field>
                </v-col>
                <v-col>
                    <v-text-field v-model="capacity" label="Capacidade do tanque (L)"></v-text-field>
                </v-col>
                <v-col>
                    <v-text-field v-model="autonomy" label="Autonomia (km)"
                        hint="Distância máxima que pode ser percorrida com o tanque cheio"></v-text-field>
                </v-col>
                <v-col>
                    <v-text-field v-model="fuelCost" label="Custo do combustível (R$)"
                        hint="Custo total do combustível para percorrer a distância informada"></v-text-field>
                </v-col>
            </v-card-text>
        </v-card>
    </v-container>
</template>

<script>
export default {
    name: 'FuelCalculator',
    data() {
        return {
            distance: 100,
            price: 5.5,
            consumption: 15,
            capacity: 45,
        };
    },
    computed: {
        autonomy() {
            return this.consumption && this.capacity ? Math.round(this.consumption * this.capacity) : 0;
        },
        fuelCost() {
            return this.distance && this.consumption && this.price ? (this.distance / this.consumption) * this.price : 0;
        },
    },
};
</script>
