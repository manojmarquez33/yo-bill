<template>
  <h1>YoBill - A Powerful Billing Software</h1>

  <div v-if="bills.length == 0">No Bill Found</div>

  <div v-else>
    <table border="2">
      <thead>
        <tr>
          <th>Bill Number</th>
          <th>Date</th> 
          <th>Customer Name</th>
          <th>Amount</th>
          <th>Status</th>
          <th colspan="3">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="bill in bills" :key="bill.id">
          <td>{{ bill.bill_number}}</td>
          <td> {{bill.bill_date }}</td>
          <td> {{bill.customer_name}}</td>
          <td>{{ bill.total_amount }}</td>
          <td>{{bill.status }}</td>
          <td><button>View</button></td>
          <td><button @click="editBill(bill)">Edit</button></td>
          <td><button @click="deleteBill(bill.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>

import { defineProps, defineEmits} from 'vue';

defineProps({

  bills : {
    type : Array,
    required : true
  },
  
});

const emit = defineEmits(['edit', 'delete']);


function editBill(bill) {
  emit('edit', bill);
}

function deleteBill (id){
  emit('delete', id);
}

</script>