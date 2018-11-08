<template>
  <div>
    <div>
      Quantity:
      <input v-model="bindedQuantity" type="number" />
    </div>
    <div>
      Description:
      <input v-model="bindedDescription" type="description" />
    </div>
    <div>
      Price:
      <input v-model="bindedPrice" type="number" />
    </div>
    <div>
      Total: {{ bindedTotal }}
    </div>
    <button v-if="!editMode" v-on:click="updateInvoiceList"> +ADD </button>
  </div>
</template>

<script>

export default {
  name: 'InvoiceInputs',
  props: {
    invoiceList: {
      type: Array,
      default: []
    },
    editMode: {
      type: Boolean,
      default: false
    },
    quantity: {
      type: Number,
      default: 0
    },
    description: {
      type: String,
      default: ''
    },
    price: {
      type: Number,
      default: 0
    },
    total: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      bindedQuantity: this.quantity,
      bindedDescription: this.description,
      bindedPrice: this.price,
      bindedTotal: this.total
    }
  },
  methods: {
    updateInvoiceList() {
      const { bindedQuantity: quantity, bindedDescription: description, bindedPrice: price } = this;
      this.bindedTotal = quantity * price;
      this.$emit("updateInvoiceList", { quantity, description, price, total: this.bindedTotal })
    }
  },
  mounted() {
    const { quantity, description, price } = this;
    console.log(quantity, description, price)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
