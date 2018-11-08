<template>
  <div>
    <div>
      Quantity:
      <input v-model="quantity" type="number" />
    </div>
    <div>
      Description:
      <input v-model="description" type="description" />
    </div>
    <div>
      Price:
      <input v-model="price" type="number" />
    </div>
    <div>
      Total: {{ total }}
    </div>
    <button v-if="!selectedInvoice" v-on:click="updateInvoiceList"> +ADD </button>
  </div>
</template>

<script>

export default {
  name: 'InvoiceInputs',
  props: ["invoiceList", "selectedInvoice"],
  data() {
    return {
      quantity: 0,
      description: '',
      price: 0,
      total: 0
    }
  },
  methods: {
    updateInvoiceList() {
      const { quantity, description, price } = this;
      this.total = quantity * price;
      this.$emit("updateInvoiceList", { quantity, description, price, total: this.total })
    }
  },
  mounted() {
    console.log(this.selectedInvoice)
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
