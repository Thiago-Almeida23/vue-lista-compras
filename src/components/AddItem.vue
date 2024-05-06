<template>
    <div class="add-item">
        <h2 class="text-center">Adicionar Novo Item</h2>
        <form @submit.prevent="addItem">
            <div class="row my-3">
                <div class="col-12">
                    <label para="itemName">Nome do Item</label>
                    <input type="text" v-model="itemName" class="form-control" required />
                </div>
            </div>

            <div class="row my-2">
                <div class="col-6">
                    <label para="itemPrice">Preço</label>
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
        };
    },
    methods: {
        addItem() {
            const price = parseFloat(this.itemPrice.replace(',', '.'));
            const newItem = {
                name: this.itemName,
                price,
                quantity: 1,
            };

            this.$emit('item-added', newItem);

            this.itemName = '';
            this.itemPrice = '';
        },

        formatPrice() {
            if (this.itemPrice === '') return;

            let rawValue = this.itemPrice.replace(/[^\d]/g, '');

            const wholePart = rawValue.slice(0, -2);
            const decimalPart = rawValue.slice(-2);
            const formattedValue = `${wholePart},${decimalPart}`;

            this.itemPrice = formattedValue;
        },
    },
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