<template>
  <div class="center-container">
    <h1>Calculadora de Propinas</h1>

    <div>
      <label>Monto de la cuenta: $</label>
      <input v-model="billAmount" type="number" />
    </div>

    <div>
      <label>Porcentaje de propina predeterminado:</label>
      <input v-model="defaultTipPercentage" type="number" />
      <span>%</span>
    </div>

    <div>
      <label>Cantidad de personas:</label>
      <input v-model="numberOfPeople" type="number" />
    </div>

    <div>
      <label>Redondear:</label>
      <select v-model="roundingOption">
        <option value="ninguno">Ninguno</option>
        <option value="total">Total</option>
        <option value="porPersona">Por Persona</option>
      </select>
    </div>

    <button @click="calculateTip">Calcular Propina</button>

    <div v-if="totalTip !== null" class="center-content">
  <p>Monto de la propina: ${{ formatCurrency(tipAmount) }}</p>
  <p>Total a pagar: ${{ formatCurrency(roundedTotal) }}</p>
  <p>Total por persona: ${{ formatCurrency(totalPerPerson) }}</p>

  <button @click="printOrSendEmail">Imprimir/Enviar por correo electrónico</button>
  </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      billAmount: 0,
      defaultTipPercentage: 15,
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
  },
  methods: {
    calculateTip() {
      const parsedBill = parseFloat(this.billAmount);
      const parsedTipPercentage = parseFloat(this.defaultTipPercentage);

      if (isNaN(parsedBill) || isNaN(parsedTipPercentage) || parsedBill <= 0 || parsedTipPercentage < 0) {
        alert('Por favor, ingrese valores válidos.');
        return;
      }

      this.totalTip = this.tipAmount;
    },
    printOrSendEmail() {
      alert("Imprimir o enviar por correo electrónico...");
    },
    formatCurrency(value) {
      if (value === undefined || value === null || isNaN(value)) {
        return '';
      }

      return parseFloat(value).toFixed(2);
    },
  },
};
</script>

<style src="../scss/main.scss" scoped lang="scss"></style>

