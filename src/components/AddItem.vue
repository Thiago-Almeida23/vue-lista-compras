<template>
    <div class="add-item">
      <h2 class="text-center">Adicionar Novo Item</h2>
      <form @submit.prevent="addItem">
        <div class="row my-3">
          <div class="col-12">
            <label for="itemName">Nome do Item</label>
            <input
              type="text"
              v-model="itemName"
              class="form-control"
              @input="showSuggestions"
              required
              list="suggestions"
            />
            <datalist id="suggestions">
              <option
                v-for="(suggestion, index) in filteredSuggestions"
                :key="index"
                :value="suggestion"
              />
            </datalist>
          </div>
        </div>
  
        <div class="row my-2">
          <div class="col-6">
            <label for="itemPrice">Preço</label>
            <input type="text" v-model="itemPrice" class="form-control" @input="formatPrice" required />
          </div>
  
          <div class="col-6 text-right">
            <button type="submit" class="btn btn-primary" style="margin-top: 24px;">
              Adicionar Item
            </button>
          </div>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        itemName: '',
        itemPrice: '',
        purchasedItems: [], // Armazena todos os nomes de itens comprados anteriormente
        filteredSuggestions: [] // Sugestões filtradas com base na entrada do usuário
      };
    },
    mounted() {
      // Carrega os itens comprados previamente do localStorage
      const storedItems = JSON.parse(localStorage.getItem('purchasedItems')) || [];
      this.purchasedItems = storedItems;
    },
    methods: {
      addItem() {
        const price = parseFloat(this.itemPrice.replace(',', '.'));
        const newItem = {
          name: this.itemName,
          price,
          quantity: 1
        };
  
        this.$emit('item-added', newItem);
  
        // Salva o item no localStorage se ainda não estiver na lista
        if (!this.purchasedItems.includes(newItem.name)) {
          this.purchasedItems.push(newItem.name);
          localStorage.setItem('purchasedItems', JSON.stringify(this.purchasedItems));
        }
  
        this.itemName = '';
        this.itemPrice = '';
        this.filteredSuggestions = [];
      },
      formatPrice() {
        if (this.itemPrice === '') return;
  
        let rawValue = this.itemPrice.replace(/[^\d]/g, '');
        const wholePart = rawValue.slice(0, -2);
        const decimalPart = rawValue.slice(-2);
        const formattedValue = `${wholePart},${decimalPart}`;
        this.itemPrice = formattedValue;
      },
      showSuggestions() {
        // Filtra as sugestões com base na entrada atual do usuário
        const input = this.itemName.toLowerCase();
        this.filteredSuggestions = this.purchasedItems.filter(item =>
          item.toLowerCase().startsWith(input)
        );
      }
    }
  };
  </script>
  
  <style scoped>
  .add-item {
    margin: 20px;
  }
  
  button {
    background-color: #007bff;
    color: #fff;
  }
  
  .col-6 {
    flex: 0 0 50%;
  }
  </style>
  