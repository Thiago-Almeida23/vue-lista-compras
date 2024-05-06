<template>
    <div class="total-value">
      <h3 class="text-center">Valor Total: R$ {{ totalValue.toFixed(2) }}</h3>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      items: {
        type: Array,
        required: true,
      },
    },
    computed: {
      totalValue() {
        return this.items.reduce((total, item) => total + item.price * item.quantity, 0);
      },
    },
  };
  </script>
  
  <style scoped>
  .total-value {
    padding: 10px;
  }
  </style>
  