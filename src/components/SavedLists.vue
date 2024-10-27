<template>
  <div class="saved-lists">
    <div v-if="lists.length">
      <ul>
        <li v-for="(list, index) in lists" :key="index">
          <h4 @click="toggleList(index)" class="accordion-header">
            Mercado: {{ list.market }} - {{ list.date }}
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
      openListIndex: null, // Para controlar qual lista está aberta
    };
  },
  methods: {
    toggleList(index) {
      // Alterna a lista aberta
      this.openListIndex = this.openListIndex === index ? null : index;
    },
    isOpen(index) {
      // Verifica se a lista está aberta
      return this.openListIndex === index;
    },
    deleteList(index) {
      // Emite um evento para excluir a lista correspondente
      this.$emit('delete-list', index);
    },
  },
};
</script>

<style scoped>
.saved-lists {
  margin-top: 20px;
}

.saved-lists ul {
  list-style-type: none; /* Remove a bolinha antes da lista */
  padding: 0; /* Remove o padding padrão da lista */
}

.saved-lists li {
  margin-bottom: 10px; /* Espaçamento entre as listas salvas */
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
  padding: 10px; /* Espaçamento interno para o conteúdo */
  border: 1px solid #ddd; /* Borda para o conteúdo expandido */
  border-top: none; /* Remove a borda superior */
}

.accordion-toggle {
  margin-left: 10px; /* Espaçamento entre o texto e o ícone */
}
</style>
