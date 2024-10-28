<template>
  <div class="container">
    <h1>Lista de Compras</h1>

    <div class="market-info">
      <label for="market">Mercado:</label>
      <input
        v-model="currentMarket"
        type="text"
        class="form-control"
        @input="showMarketSuggestions"
        list="marketSuggestions"
        required
      />
      <!-- Lista de sugestões para o nome do mercado -->
      <datalist id="marketSuggestions">
        <option v-for="(suggestion, index) in filteredMarketSuggestions" :key="index" :value="suggestion" />
      </datalist>
    </div>

    <AddItem @item-added="handleItemAdded" />

    <ItemList :items="shoppingList" />

    <TotalValue :items="shoppingList" />

    <div class="save-list-button">
      <button @click="saveList" class="btn btn-success mt-4">Salvar Lista</button>
    </div>

    <h2 class="mt-5">Listas Salvas</h2>
    <SavedLists :lists="savedLists" @delete-list="removeList" />
  </div>
</template>

<script>
import AddItem from './components/AddItem.vue';
import ItemList from './components/ItemList.vue';
import TotalValue from './components/TotalValue.vue';
import SavedLists from './components/SavedLists.vue';

export default {
  components: {
    AddItem,
    ItemList,
    TotalValue,
    SavedLists,
  },
  data() {
    return {
      shoppingList: [],
      savedLists: [],
      currentMarket: '',
      markets: [], // Armazena os mercados já registrados
      filteredMarketSuggestions: [], // Sugestões de mercado com base no input
    };
  },
  methods: {
    handleItemAdded(newItem) {
      this.shoppingList.push(newItem);
    },
    saveList() {
      if (!this.currentMarket) {
        alert("Por favor, insira o nome do mercado.");
        return;
      }
      const date = new Date().toLocaleDateString();
      const newList = {
        market: this.currentMarket,
        items: [...this.shoppingList],
        date,
      };

      // Salva o nome do mercado no localStorage se ainda não estiver na lista
      if (!this.markets.includes(this.currentMarket)) {
        this.markets.push(this.currentMarket);
        localStorage.setItem('markets', JSON.stringify(this.markets));
      }

      this.savedLists.push(newList);
      localStorage.setItem('savedLists', JSON.stringify(this.savedLists));
      this.shoppingList = [];
      this.currentMarket = '';
      alert("Lista salva com sucesso!");
    },
    loadLists() {
      const saved = localStorage.getItem('savedLists');
      this.savedLists = saved ? JSON.parse(saved) : [];

      // Carrega os mercados previamente salvos do localStorage
      const storedMarkets = JSON.parse(localStorage.getItem('markets')) || [];
      this.markets = storedMarkets;
    },
    removeList(index) {
      console.log('Removendo a lista na posição:', index);
      this.savedLists.splice(index, 1);
      localStorage.setItem('savedLists', JSON.stringify(this.savedLists));
      console.log('Listas salvas após remoção:', this.savedLists);
    },
    showMarketSuggestions() {
      const input = this.currentMarket.toLowerCase();
      this.filteredMarketSuggestions = this.markets.filter(market =>
        market.toLowerCase().startsWith(input)
      );
    },
  },
  mounted() {
    this.loadLists();
  },
};
</script>

<style scoped>
.container {
  padding: 50px 5px;
  width: 100%;
  border: 3px solid #262626;
  border-radius: 5px;
  background-color: #c3c0c0;
}

h1, h2 {
  text-align: center;
}

.market-info {
  margin: 10px 0;
  display: flex;
  align-items: center;
  margin-bottom: 15px;
}

.market-info label {
  margin-left: 20px;
  margin-right: 10px;
  white-space: nowrap;
}

.market-info .form-control {
  width: 200px;
  flex: none; 
}

.save-list-button {
  display: flex;
  justify-content: center; 
  margin-top: 20px;
}
</style>
