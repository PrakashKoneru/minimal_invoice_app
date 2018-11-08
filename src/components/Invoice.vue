<template>
  <div>
    <table>
      <tr>
        <th>Quantity</th>
        <th>Description</th>
        <th>Price</th>
        <th>Total</th>
      </tr>
      <tr v-for="(invoice, index) in invoiceList" v-on:click="editInvoice(index)" :key="index">
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
      this.invoiceList.push(value)
    },
    deleteInvoice(index) {
      this.invoiceList = this.invoiceList.filter((invoice, i) => i !== index);
    },
    editInvoice(index) {
      this.selectedInvoice = this.invoiceList[index];
      this.$modal.show('InvoiceModal');
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