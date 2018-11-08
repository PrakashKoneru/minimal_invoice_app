<template>
  <div>
    <div>
      Quantity:
      <input v-model.number="bindedQuantity" type="number" />
    </div>
    <div>
      Description:
      <input v-model="bindedDescription" type="description" />
    </div>
    <div>
      Price:
      <input v-model.number="bindedPrice" type="number" />
    </div>
    <div>
      Total: {{ bindedTotal }}
    </div>
    <button v-if="!editMode" v-on:click="updateInvoiceList"> +ADD </button>
    <button v-if="editMode" v-on:click="editInvoice"> Edit </button>
    <button v-if="editMode" v-on:click="deleteInvoice"> Delete </button>
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
    },
    _id: {
      type: Number
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
      this.$emit("updateInvoiceList", [...this.invoiceList, {
        quantity,
        description,
        price,
        total: this.bindedTotal,
        _id: this.invoiceList.length
      }]);
    },
    editInvoice() {
      const { bindedQuantity: quantity, bindedDescription: description, bindedPrice: price } = this;
      this.bindedTotal = quantity * price;
      this.$emit("editInvoice", {
        quantity,
        description,
        price,
        total: this.bindedTotal,
        _id: this._id
      });
    },
    deleteInvoice() {
      this.$emit("deleteInvoice")
    }
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
