
<script setup>

import {ref,watchEffect} from  'vue';
import BillList from './components/BillList.vue';
import BillForm from './components/BillForm.vue';


// export default{
//   name : 'App',
//   components : {
//     //BillList
//   },
//   data (){
//     return{
//       typeWriters : 10,
//     }
//   },
//   methods:{
//     remove(){
//       this.typeWriters--;
//       console.log(typeWriters);
//     }
//   }
// }


const defaultBill = {
  id: null,
  bill_number: 'YB',
  bill_date: '',
  customer_name: '',
  customer_email: '',
  total_amount: 0,
  items_description: '',
  status: 'Pending'
};

const bills = ref([]);

const currentBill = ref({ ...defaultBill });

const showForm = ref(false);

watchEffect(() => {

try {
  
  const storedBills = localStorage.getItem('yobill-bills');

  if(storedBills){
    bills.value = JSON.parse(storedBills);
  } else {
     bills.value = [
        { id: 1, bill_number: 'YA01', bill_date: '2025-01-15', customer_name: 'Jegan', customer_email: 'jegan@gmail.com', total_amount: 150, items_description: 'Product A, Service B', status: 'Paid' },
        { id: 2, bill_number: 'YA02', bill_date: '2025-01-20', customer_name: 'Deepak', customer_email: 'deepak@gmail.com', total_amount: 200, items_description: 'Product C', status: 'Pending' },
        { id: 3, bill_number: 'YA03', bill_date: '2025-01-25', customer_name: 'Bala', customer_email: 'bala@gmail.com', total_amount: 75, items_description: 'Product D', status: 'Overdue' }
      ];
      localStorage.setItem('yobill-bills', JSON.stringify(bills.value));
  }
} catch (error) {
  console.log('catch error :',error);
  bills.value = [];
}
});


// localStorage.setItem('yobill-bills',JSON.stringify(bills.value));

watchEffect(() => {
  try{
    localStorage.setItem('yobill-bills',JSON.stringify(bills.value));
    // console.log(showForm.value);
    // console.log(currentBill.value);
  } catch(e){
    console.error("failed",e);
  }
});


function saveBill (billData) {
  if(billData.id){
    const index = bills.value.findIndex(b => b.id == billData.id);
    if(index != -1){
      bills.value[index] = { ...billData};
    }

  } else {
    billData.id = Date.now();
    console.log("id:",billData.id);

    bills.value.push({...billData});
  }

  currentBill.value = { ...defaultBill};
  showForm.value = false;
}

function editBill(bill){
  currentBill.value = { ...bill};
  showForm.value = true;
}

function deleteBill(id){
  bills.value = bills.value.filter(bill => bill.id != id);
}

function cancelEdit() {
  currentBill.value = {defaultBill};
  showForm.value = false;
}

function addNewBill () {
currentBill.value = { ...defaultBill};
showForm.value = true;
}


</script>

<template>
    <img alt="Vue logo" src="./assets/logo.png" width="150" />

    <div v-if="!showForm">
      <button v-if="!showForm" @click="addNewBill">Add New Bill</button>
    </div>

    <BillForm v-if="showForm" 
    :bill="currentBill"  
    @save="saveBill" 
    @cancel="cancelEdit"
    />
    
    <TabelList />
    
    <BillList 
    v-if="!showForm"
    :bills="bills"
    @edit="editBill"
    @delete="deleteBill"/>
    
   
</template>
