<template>
  <div>
    <div class="invoiceTable">
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
    </div>
    <div class="invoiceModal">
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
      this.invoiceList = this.invoiceList.map((invoice) => {
        if (invoice._id === this.selectedInvoice._id) {
          return value
        }
        return invoice
      });
      this.$modal.hide('InvoiceModal');
    },
    deleteInvoice(index) {
      this.invoiceList = this.invoiceList.filter((invoice, i) => i !== index);
    }
  }
}
</script>

<style scoped>
  .invoiceTable {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0 20px;
  }
  table {
    width: 100%;
    max-width: 600px;
    border-collapse: collapse;
  }
  td, th {
    border: 1px solid black;
    padding: 10px;
    cursor: pointer;
  }
</style>