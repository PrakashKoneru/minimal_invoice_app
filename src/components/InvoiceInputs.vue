<template>
  <div>
    <div class="invoiceInputs">
      <div>
        <div>Quantity:</div>
        <input v-model.number="bindedQuantity" type="number" />
      </div>
      <div>
        <div>Description:</div>
        <div><input v-model="bindedDescription" type="description" /></div>
      </div>
      <div>
        <div>Price:</div>
        <input v-model.number="bindedPrice" type="number" />
      </div>
    </div>
    <div class="invoiceButtons">
      <button v-if="!editMode" v-on:click="updateInvoiceList"> +ADD </button>
      <button v-if="editMode" v-on:click="updateInvoice"> Update </button>
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
    updateInvoice() {
      const { bindedQuantity: quantity, bindedDescription: description, bindedPrice: price } = this;
      this.bindedTotal = quantity * price;
      this.$emit("updateInvoice", {
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
    margin: auto;
    margin-top: 20px;
    max-width: 85%;
  }

  .invoiceInputs > div {
    margin-top: 20px;
  }

  input {
    height: 25px;
    padding: 2px 5px;
    outline: none;
    width: 100%;
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

  @media (min-width: 600px) {
    .invoiceInputs {
      display: flex;
      justify-content: space-around;
    }
  }
</style>
