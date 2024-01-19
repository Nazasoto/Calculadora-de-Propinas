<template>
  <div id="app">
    <TipCalculator />
    <PrintAndEmailButton />
  </div>
</template>

<script>
import TipCalculator from './components/TipCalculator.vue';
import PrintAndEmailButton from './components/PrintAndEmailButton.vue';

export default {
  components: {
    TipCalculator,
    PrintAndEmailButton
}
}
</script>


