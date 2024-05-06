<template>
  <div class="container">
    <h1>Lista de Compras</h1>

    <AddItem @item-added="handleItemAdded" />

    <ItemList :items="shoppingList" />

    <TotalValue :items="shoppingList" />
  </div>
</template>

<script>
import AddItem from './components/AddItem.vue';
import ItemList from './components/ItemList.vue';
import TotalValue from './components/TotalValue.vue';

export default {
  components: {
    AddItem,
    ItemList,
    TotalValue,
  },
  data() {
    return {
      shoppingList: [],
    };
  },
  methods: {
    handleItemAdded(newItem) {
      this.shoppingList.push(newItem);
    },
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

h1 {
  text-align: center;
}
</style>
