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
      <div v-if="invoiceList.length !== 0" class="total">Total: {{ computedTotal }}</div>
    </div>
    <div class="invoiceModal">
      <InvoiceInputs :invoiceList="invoiceList" @updateInvoiceList="updateInvoiceList"/>
      <InvoiceModal name="InvoiceModal">
        <InvoiceInputs
          :editMode="true"
          :invoiceList="invoiceList"
          v-bind="selectedInvoice"
          @deleteInvoice="deleteInvoice"
          @updateInvoice="updateInvoice"
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
  computed: {
    computedTotal: function () {
      let count = 0;
      for(let i=0; i<this.invoiceList.length; i++) {
        count += this.invoiceList[i].total;
      }
      return count;
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
    updateInvoice(value) {
      this.invoiceList = this.invoiceList.map((invoice) => {
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

<style scoped>
  .invoiceTable {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 0 20px;
  }
  .total {
    width: 100%;
    max-width: 600px;
    margin-top: 15px;
    font-weight: bold;
    font-size: 18px;
    text-align: right;
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