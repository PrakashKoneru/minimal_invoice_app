<template>
  <div>
    <table>
      <tr>
        <th>Quantity</th>
        <th>Description</th>
        <th>Price</th>
        <th>Total</th>
      </tr>
      <tr v-for="(invoice, index) in invoiceList" v-on:click="showModalFor(index)" :key="invoice._id">
        <td>{{ invoice.quantity }}</td>
        <td>{{ invoice.description }}</td>
        <td>{{ invoice.price }}</td>
        <td>{{ invoice.total }}</td>
      </tr>
    </table>
    <InvoiceInputs :invoiceList="invoiceList" @updateInvoiceList="updateInvoiceList"/>
    <InvoiceModal name="InvoiceModal">
      <InvoiceInputs
        :editMode="true"
        :invoiceList="invoiceList"
        v-bind="selectedInvoice"
        @deleteInvoice="deleteInvoice"
        @editInvoice="editInvoice"
        @updateInvoiceList="updateInvoiceList"
      />
    </InvoiceModal>
  </div>
</template>

<script>
import InvoiceInputs from './InvoiceInputs.vue';

export default {
  name: 'Invoice',
  components: {
    InvoiceInputs
  },
  data() {
    return {
      invoiceList: [],
      selectedInvoice: {}
    }
  },
  methods: {
    updateInvoiceList(value) {
      console.log(value)
      this.invoiceList = value
    },
    deleteInvoice() {
      this.invoiceList = this.invoiceList.filter((invoice) => invoice._id !== this.selectedInvoice._id);
      this.$modal.hide('InvoiceModal');
    },
    showModalFor(index) {
      this.selectedInvoice = this.invoiceList[index];
      this.$modal.show('InvoiceModal');
    },
    editInvoice(value) {
      this.invoiceList = this.invoiceList.map((invoice, index) => {
        if (invoice._id === this.selectedInvoice._id) {
          return value
        }
        return invoice
      });
      this.$modal.hide('InvoiceModal');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  table {
    border-collapse: collapse;
  }
  td, th {
    border: 1px solid black;
    padding: 10px;
    width: 100px;
  }
</style>