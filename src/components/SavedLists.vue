<template>
  <div class="saved-lists">
    <div v-if="lists.length">
      <ul>
        <li v-for="(list, index) in lists" :key="index">
          <h4 @click="toggleList(index)" class="accordion-header">
            {{ list.market }} - {{ list.date }} - Total: R$ {{ getTotal(list).toFixed(2) }}
            <span class="accordion-toggle">{{ isOpen(index) ? '-' : '+' }}</span>
          </h4>
          <div v-if="isOpen(index)" class="accordion-content">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>Produto</th>
                  <th>Preço</th>
                  <th>Quantidade</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, itemIndex) in list.items" :key="itemIndex">
                  <td>{{ item.name }}</td>
                  <td>R$ {{ item.price.toFixed(2) }}</td>
                  <td>{{ item.quantity }}</td>
                </tr>
              </tbody>
            </table>
            <button @click="deleteList(index)" class="btn btn-danger mt-2">Excluir Lista</button>
          </div>
        </li>
      </ul>
    </div>
    <p v-else>Nenhuma lista salva.</p>
  </div>
</template>

<script>
export default {
  props: {
    lists: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      openLists: [], // Array para controlar quais listas estão abertas
    };
  },
  methods: {
    toggleList(index) {
      this.openLists[index] = !this.openLists[index]; // Alterna o estado de abertura da lista
    },
    isOpen(index) {
      return !!this.openLists[index];
    },
    deleteList(index) {

      this.$emit('delete-list', index);
    },
    getTotal(list) {
      return list.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }
  },
};
</script>

<style scoped>
.saved-lists {
  margin-top: 20px;
}

.saved-lists ul {
  list-style-type: none;
  padding: 0;
}

.saved-lists li {
  margin-bottom: 10px;
}

.accordion-header {
  cursor: pointer; 
  background-color: #f8f9fa;
  padding: 10px; 
  border: 1px solid #ddd;
  border-radius: 4px; 
  display: flex; 
  justify-content: space-between;
}

.accordion-content {
  padding: 10px;
  border: 1px solid #ddd;
  margin-top: 5px;
}
</style>
