<script setup>
import { ref, watch, defineProps, defineEmits } from "vue";

const props = defineProps({
  bill: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["save", "cancel"]);

const formData = ref({ ...props.bill });

watch(
  () => props.bill,
  (newVal) => {
    formData.value = { ...newVal };
  }
);


function handleCancel(){
  emit("cancel");
}

function handleSubmit(){
  emit("save",formData.value);
}
</script>
<template>
  <div>
    <h2>
      {{ formData.id ? "Edit Bill" : "Add New Bill" }}
    </h2>

    <form @submit.prevent="handleSubmit">
      <div>
        <label for="bill_number">Bill Number</label>
        <input
          type="text"
          id="bill_number"
          v-model="formData.bill_number"
          required
        />
      </div>
      <div>
        <label for="bill_date">Bill Date</label>
        <input
          type="date"
          id="bill_date"
          v-model="formData.bill_date"
          required
        />
      </div>
      <div>
        <label for="customer_name">Customer Name</label>
        <input
          type="text"
          id="customer_name"
          v-model="formData.customer_name"
          required
        />
      </div>
      <div>
        <label for="customer_email">Customer Email</label>
        <input
          type="text"
          id="customer_email"
          v-model="formData.customer_email"
          required
        />
      </div>
      <div>
        <label for="total_amount">Total Amount</label>
        <input
          type="number"
          id="total_amount"
          v-model="formData.total_amount"
          required
          min="0"
        />
      </div>
      <div>
        <label for="status">Status:</label>
        <select id="status" v-model="formData.status">
          <option value="Pending">Pending</option>
          <option value="Paid">Paid</option>
          <option value="Overdue">Overdue</option>
          <option value="Cancelled">Cancelled</option>
        </select>
      </div>
      <div>
        <label for="items_description">Items Description:</label>
        <textarea
          id="items_description"
          v-model="formData.items_description"
          rows="4"
        ></textarea>
      </div>
      <div>
        <button type="button" @click="handleCancel">Cancel</button>
      </div>
      <div>
        <button type="submit">
          {{ formData.id ? "Update Bill" : "Add Bill" }}
        </button>
      </div>
    </form>
  </div>
</template>
<style scoped>
</style>