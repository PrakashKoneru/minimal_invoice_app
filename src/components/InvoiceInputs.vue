<template>
  <div>
    <div class="invoiceInputs">
      <div class="invoiceField">
        <div>Quantity:</div>
        <input v-model.number="bindedQuantity" type="number" />
      </div>
      <div class="invoiceField">
        <div>Description:</div>
        <input v-model="bindedDescription" type="description" />
      </div>
      <div class="invoiceField">
        <div>Price:</div>
        <input v-model.number="bindedPrice" type="number" />
      </div>
      <div class="invoiceField">
        <div>Total:</div>
        <div>{{ bindedTotal }}</div>
      </div>
    </div>
    <div class="invoiceButtons">
      <button v-if="!editMode" v-on:click="updateInvoiceList"> +ADD </button>
      <button v-if="editMode" v-on:click="editInvoice"> Edit </button>
      <button v-if="editMode" v-on:click="deleteInvoice"> Delete </button>
      <button v-if="editMode" v-on:click="cancelModal"> Cancel </button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'InvoiceInputs',
  props: {
    invoiceList: {
      type: Array,
      default: () => []
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
    },
    cancelModal() {
      this.$modal.hide('InvoiceModal');
    }
  }
}
</script>

<style scoped>
  .invoiceInputs {
    display: flex;
    justify-content: space-around;
    margin: auto;
    margin-top: 40px;
    flex-wrap: wrap;
    max-width: 80%;
    text-align: left;
  }

  input {
    height: 25px;
    padding: 2px 5px;
    outline: none;
  }

  input[type="number"]::-webkit-outer-spin-button, input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .invoiceButtons {
    margin-top: 20px;
    text-align: center;
  }
  
  button {
    border: 1px solid initial;
    border-radius: 3px;
    padding: 4px 10px;
    cursor: pointer;
    min-width: 80px;
    min-height: 25px;
    outline: none;
    margin: 0 10px;
  }
</style>
