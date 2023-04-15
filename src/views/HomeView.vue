<template>
  <FuelCalculator />
</template>

<script>
import { defineComponent } from 'vue';

// Components
import FuelCalculator from '../components/FuelCalculator.vue';

export default defineComponent({
  name: 'HomeView',

  components: {
    FuelCalculator,
  },
});
</script>
