<script setup>
import { reactive, ref } from "vue";

const data = reactive({
  invoiceNumber: "",
  date: "",
  dueDate: "",
  additionalNote: "",
  sender: {
    name: "",
    address: "",
    email: "",
    phone: "",
  },
  recipient: {
    name: "",
    address: "",
    email: "",
    phone: "",
  },
  billTo: "",
  shipTo: "",
  Items: [{ description: "", quantity: 1, rate: 0, amount: 0 }],
  taxRate: 10,
  discount: 0,
  shipping: 0,
  notes: "",
  terms: "",
  subtotal: "",
  total: "",
});
const lineItems = ref([{ description: "", quantity: 1, rate: 0 }]);

const addLineItem = () => {
  lineItems.value.push({ description: "", quantity: 1, rate: 0 });
};

const calculateAmount = (item) => item.quantity * item.rate;

const calculateSubtotal = () => {
  return lineItems.value.reduce(
    (total, item) => total + calculateAmount(item),
    0
  );
};

const taxRate = ref(0);

const calculateTotal = () => {
  const subtotal = calculateSubtotal();
  return subtotal + (subtotal * taxRate.value) / 100;
};
</script>

<template>
  <div class="p-10 bg-gray-50 min-h-screen">
    <div class="max-w-4xl mx-auto bg-white p-8 shadow-lg rounded-lg">
      <!-- Header -->
      <div class="flex items-center justify-between mb-6">
        <div>
          <label class="block mb-2 text-sm font-semibold">Add Your Logo</label>
          <div
            class="w-32 h-32 bg-gray-200 flex items-center justify-center text-gray-400"
          >
            + Add Your Logo
          </div>
        </div>
        <div class="text-right">
          <h1 class="text-3xl font-bold">INVOICE</h1>
          <input
            type="text"
            placeholder="#"
            class="mt-4 w-32 border-b-2 border-gray-300 focus:outline-none text-center"
          />
        </div>
      </div>

      <!-- Sender and Recipient Info -->
      <div class="grid grid-cols-9 gap-8 justify-between font-semibold">
        <div class="col-span-5 mb-6">
          <div class="flex flex-col">
            <label>Sender</label>
            <input
              type="text"
              class="border border-gray-300 rounded-lg p-2"
              v-model="data.sender.name"
            />
          </div>
          <div class="grid grid-cols-2 gap-2">
            <div class="flex flex-col col-span-1">
              <label>Bill To</label>
              <input
                type="text"
                class="border border-gray-300 rounded-lg p-2"
                v-model="data.recipient.name"
              />
            </div>
            <div class="flex flex-col col-span-1">
              <label>Ship To</label>
              <input
                type="text"
                class="border border-gray-300 rounded-lg p-2"
                v-model="data.recipient.name"
              />
            </div>
          </div>
        </div>
        <div class="col-span-4">
          <!-- Invoice Details -->
          <div class="flex flex-col gap-4 mb-6">
            <div class="flex space-x-0 items-center">
              <label class="w-1/3 text-right pr-2">Date</label>
              <input
                type="text"
                class="border border-gray-300 rounded-lg p-2"
                v-model="data.recipient.name"
              />
            </div>
            <div class="flex items-center">
              <label class="w-1/3 text-right pr-2">Due Date</label>
              <input
                type="text"
                class="border border-gray-300 rounded-lg p-2"
                v-model="data.recipient.name"
              />
            </div>
            <div class="flex items-center">
              <label class="w-1/3 text-right pr-2">Additional Note</label>
              <input
                type="text"
                class="border border-gray-300 rounded-lg p-2"
                v-model="data.recipient.name"
              />
            </div>

            <input
              type="text"
              placeholder="PO Number"
              class="border border-gray-300 rounded-lg p-2"
            />
          </div>
        </div>
      </div>
   <!-- Line Items -->
   <div class="border-t-2 border-gray-300 mt-6 pt-4">
        <div class="flex items-center space-x-4 mb-2 font-semibold bg-gray-900 text-white px-2">
          <div class="w-2/5">Item</div>
          <div class="w-1/5">Quantity</div>
          <div class="w-1/5">Rate</div>
          <div class="w-1/5 text-right">Amount</div>
        </div>
        <div v-for="(item, index) in lineItems" :key="index" class="flex items-center space-x-4 mb-2">
          <input
            type="text"
            placeholder="Description of service or product..."
            class="w-2/5 border border-gray-300 rounded-lg p-2"
            v-model="item.description"
          />
          <input
            type="number"
            placeholder="Qty"
            class="w-1/5 border border-gray-300 rounded-lg p-2 text-center"
            v-model="item.quantity"
          />
          <input
            type="number"
            placeholder="Rate"
            class="w-1/5 border border-gray-300 rounded-lg p-2 text-center"
            v-model="item.rate"
          />
          <div class="w-1/5 text-right">
            {{ calculateAmount(item) || currency }}
          </div>
        </div>
        <button
          @click="addLineItem"
          class="mt-4 bg-green-500 text-white py-1 px-3 rounded-lg"
        >
          Add More
        </button>
      </div>

      <!-- Summary -->
      <div class="mt-8 text-right space-y-2">
        <div>
          <span>Subtotal:</span>
          <span>{{ calculateSubtotal() || currency }}</span>
        </div>
        <div>
          <label>Tax:</label>
          <input
            type="number"
            v-model="data.taxRate"
            class="border-b-2 w-12 text-right"
            placeholder="0"
          />
          %
        </div>
        <div>
          <span>Total:</span>
          <span>{{ calculateTotal() || currency }}</span>
        </div>
      </div>

      <!-- Notes and Terms -->
      <div class="mt-6">
        <label >Notes</label>
        <textarea
          placeholder="Notes - any relevant information not already covered"
          class="w-full border border-gray-300 rounded-lg p-2 mb-4"
          rows="3"
          v-model="data.notes"
        ></textarea>
        <label >Terms</label>
        <textarea
          placeholder="Terms and conditions - late fees, payment methods, delivery schedule"
          class="w-full border border-gray-300 rounded-lg p-2"
          rows="3"
          v-model="data.terms"
        ></textarea>
      </div>
    
  </div>
  </div>
</template>

<style scoped>
/* Additional styling as needed */
</style>
