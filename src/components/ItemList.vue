<template>
  <div class="item-list">
    <div class="table-responsive" style="max-height: 300px; overflow-y: scroll;">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Produto</th>
            <th>Preço</th>
            <th>Quantidade</th>
            <th>Excluir</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in items" :key="index">
            <td>{{ item.name }}</td>
            <td>{{ item.price.toFixed(2) }}</td>
            <td>
              <div class="d-flex justify-content-end align-items-center">
                <span>{{ item.quantity }}</span>
                <button 
                  @click="increaseQuantity(index)" 
                  class="btn btn-sm btn-success"
                >
                  <i class="bi bi-plus"></i>
                </button>
                <button 
                  @click="decreaseQuantity(index)" 
                  class="btn btn-sm btn-danger"
                >
                  <i class="bi bi-dash"></i>
                </button>
              </div>
            </td>
            <td>
              <button 
                @click="removeItem(index)" 
                class="btn btn-sm btn-inline-danger"
              >
                <i class="bi bi-trash" style="color: red;"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
      validator: (value) => {
        return Array.isArray(value) && value.every(
          (item) => 'name' in item && 'price' in item && 'quantity' in item
        );
      },
    },
  },
  methods: {
    increaseQuantity(index) {
      this.items[index].quantity += 1; 
    },
    decreaseQuantity(index) {
      if (this.items[index].quantity > 1) {
        this.items[index].quantity -= 1; 
      }
    },
    removeItem(index) {
      this.items.splice(index, 1); 
    },
  },
};
</script>

<style scoped>
.item-list {
  padding: 20px;
}

.table-responsive { 
  max-height: 300px;
  overflow-y: scroll;
}

.table { 
  width: 100%; 
  margin-bottom: 20px; 
  border: 1px solid #262626; 
}

.table th, .table td {
  padding: 10px 5px;
}

.custom-button {
  height: 24px; 
  width: 24px; 
  display: flex; 
  justify-content: center; 
  align-items: center; 
}

.d-flex {
  display: flex; 
  flex-direction: row; 
  justify-content: end;
}

button {
  margin-left: 5px; 
}
</style>
