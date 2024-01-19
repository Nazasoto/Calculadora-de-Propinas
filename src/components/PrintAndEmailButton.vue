<!-- PrintAndEmailButton.vue -->

<template>
    <div>
      <button @click="printOrSendEmail">Imprimir/Enviar por correo electrónico</button>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      printOrSendEmail() {
        const printContent = `
          Monto de la propina: $${this.formatCurrency(this.$store.state.tipAmount)}
          Total a pagar: $${this.formatCurrency(this.$store.state.totalAmount)}
          Total por persona: $${this.formatCurrency(this.$store.state.totalPerPerson)}
        `;
  
        // Imprimir la página
        window.print();
  
        // Abrir cliente de correo electrónico predeterminado
        window.open(`mailto:?subject=Resumen%20de%20Propina&body=${encodeURIComponent(printContent)}`);
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
  