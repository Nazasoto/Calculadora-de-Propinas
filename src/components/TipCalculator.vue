<template>
  <div class="calculator-container">
    <h1>Calculadora de Propinas</h1>

    <div>
      <label>Monto de la cuenta: $</label>
      <input v-model="billAmount" type="text" pattern="[0-9]+" />
    </div>

    <div>
      <label>Porcentaje de propina predeterminado:</label>
      <input v-model="defaultTipPercentage" type="text" pattern="[0-9]+" />
    </div>

    <div>
      <label>Cantidad de personas:</label>
      <input v-model="numberOfPeople" type="text" pattern="[0-9]+" />
    </div>

    <div>
      <label>Redondear:</label>
      <select v-model="roundingOption">
        <option value="ninguno">Ninguno</option>
        <option value="total">Total</option>
        <option value="porPersona">Por Persona</option>
      </select>
    </div>

    <div v-if="totalTip !== null" class="center-content">
      <p>Monto de la propina: {{ formattedCurrency(tipAmount) }}</p>
      <p>Total a pagar: {{ formattedCurrency(roundedTotal) }}</p>
      <p>Total por persona: {{ formattedCurrency(totalPerPerson) }}</p>

      <button @click="printOrSendEmail" :key="someUniqueKey">Imprimir / Enviar por correo electrónico</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      billAmount: 0,
      defaultTipPercentage: 10,
      numberOfPeople: 1,
      roundingOption: 'ninguno',
      totalTip: 0,
    };
  },
  computed: {
    roundedTotal() {
      if (this.roundingOption === 'ninguno') {
        return this.totalAmount;
      } else if (this.roundingOption === 'total') {
        return Math.round(this.totalAmount);
      } else if (this.roundingOption === 'porPersona') {
        return Math.round(this.totalPerPerson * this.numberOfPeople);
      }
      return this.totalAmount; // Por defecto, no redondear
    },
    tipAmount() {
      return (parseFloat(this.billAmount) * parseFloat(this.defaultTipPercentage)) / 100;
    },
    totalAmount() {
      return parseFloat(this.billAmount) + this.tipAmount;
    },
    totalPerPerson() {
      return this.totalAmount / this.numberOfPeople;
    },
    isInvalidInput() {
      const parsedBill = parseFloat(this.billAmount);
      const parsedTipPercentage = parseFloat(this.defaultTipPercentage);
      const parsedNumberOfPeople = parseInt(this.numberOfPeople);

      return isNaN(parsedBill) || isNaN(parsedTipPercentage) || isNaN(parsedNumberOfPeople) || 
             parsedBill < 1 || parsedTipPercentage < 10 || parsedNumberOfPeople < 1;
    },
  },
  methods: {
    calculateTip() {
      const parsedBill = parseFloat(this.billAmount);
      const parsedTipPercentage = parseFloat(this.defaultTipPercentage);

      if (isNaN(parsedBill) || isNaN(parsedTipPercentage) || 
          parsedBill < 1 || parsedTipPercentage < 10) {
        alert('Por favor, ingrese valores válidos. El monto de la cuenta debe ser mayor a 1 y el porcentaje de propina no puede ser menor al 10%.');
        return;
      }

      this.totalTip = this.tipAmount;
    },
    printOrSendEmail() {
      alert("Imprimir o enviar por correo electrónico...");
    },
    formattedCurrency(value) {
      if (value === undefined || value === null || isNaN(value)) {
        return '';
      }

      return new Intl.NumberFormat('es-ES', { style: 'currency', currency: 'USD' }).format(parseFloat(value));
    },
  },
};
</script>

<style src="../scss/main.scss" scoped lang="scss"></style>
