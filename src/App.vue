<template>
  <section class="min-h-screen w-full flex items-center justify-center bg-linear-to-br from-[#073938] via-[#0a4a49] to-[#0d5c5a] p-4 font-poppins">
    <div class="w-full max-w-md bg-white/95 backdrop-blur-lg rounded-3xl shadow-2xl p-8 transition-all duration-300">
      <div class="mb-8 text-center">
        <h1 class="text-3xl font-extrabold bg-linear-to-r from-[#B3812C] to-[#d4a847] bg-clip-text text-transparent">
          Tip Calculator
        </h1>
        <p class="text-gray-600 text-sm mt-2">Split bills easily</p>
      </div>

      <div class="space-y-6">
        <div>
          <label class="block text-sm font-semibold text-[#073938] mb-2">Bill Amount ($)</label>
          <input
            v-model.number="bill"
            @input="handleBillInput"
            type="number"
            min="0"
            step="0.01"
            class="w-full h-14 rounded-xl border-2 border-[#073938]/30 p-4 text-lg focus:outline-none focus:border-[#B3812C] focus:ring-4 focus:ring-[#B3812C]/20 transition-all"
            placeholder="0.00"
          />
        </div>

        <div>
          <label class="block text-sm font-semibold text-[#073938] mb-2">Tip Percentage</label>
          <div class="grid grid-cols-3 gap-3 mb-3">
            <button
              v-for="percent in [5, 10, 15]"
              :key="percent"
              @click="selectPreset(percent)"
              :class="tipPercent === percent && !isCustom ? 'bg-[#B3812C] text-white' : 'bg-[#073938]/10 text-[#073938]'"
              class="py-3 rounded-xl font-bold transition-all hover:scale-105"
            >
              {{ percent }}%
            </button>
          </div>
          <div>
            <label class="block text-xs font-medium text-gray-500 mb-1 ml-1">Custom Tip %</label>
            <input
              v-model.number="tipPercent"
              @input="handleCustomInput"
              type="number"
              min="0"
              max="100"
              class="w-full h-12 rounded-xl border-2 border-[#073938]/30 p-3 text-center focus:outline-none focus:border-[#B3812C] focus:ring-4 focus:ring-[#B3812C]/20 transition-all"
              placeholder="Enter custom %"
            />
          </div>
        </div>

        <div>
          <label class="block text-sm font-semibold text-[#073938] mb-2">Number of People</label>
          <input
            v-model.number="people"
            type="number"
            min="1"
            class="w-full h-14 rounded-xl border-2 border-[#073938]/30 p-4 text-lg focus:outline-none focus:border-[#B3812C] focus:ring-4 focus:ring-[#B3812C]/20 transition-all"
            placeholder="1"
          />
        </div>

        <div class="bg-linear-to-br from-[#B3812C]/10 to-[#073938]/10 rounded-2xl p-6 border-2 border-[#B3812C]/30">
          <div class="grid grid-cols-2 gap-4">
            <div>
              <p class="text-[#073938] text-sm font-semibold">Tip Amount</p>
              <p class="text-2xl font-bold text-[#B3812C]">${{ tipAmount.toFixed(2) }}</p>
            </div>
            <div>
              <p class="text-[#073938] text-sm font-semibold">Total Per Person</p>
              <p class="text-2xl font-bold text-[#B3812C]">${{ totalPerPerson.toFixed(2) }}</p>
            </div>
          </div>
          <div class="mt-4 pt-4 border-t border-[#B3812C]/20">
            <p class="text-[#073938] text-sm font-semibold">Grand Total</p>
            <p class="text-3xl font-extrabold text-[#073938]">${{ grandTotal.toFixed(2) }}</p>
          </div>
        </div>

        <button
          @click="reset"
          class="w-full py-4 rounded-xl font-bold text-lg bg-[#073938] text-white hover:bg-[#0a4a49] transition-all"
        >
          🔄 Reset
        </button>
      </div>

      <div class="mt-8 pt-6 border-t border-gray-200 text-center">
        <p class="text-xs uppercase tracking-widest text-gray-400 mb-2">Coded by</p>
        <a 
          href="https://github.com/fatintawsifhoque" 
          target="_blank" 
          class="text-xl font-bold bg-linear-to-r from-[#073938] to-[#B3812C] bg-clip-text text-transparent hover:scale-110 transition-transform duration-300 inline-block cursor-pointer"
        >
          Fatin Tawsif Hoque
        </a>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue';

const bill = ref(0);
const tipPercent = ref(10);
const people = ref(1);
const isCustom = ref(false);

const tipAmount = computed(() => {
  return (bill.value * tipPercent.value) / 100;
});

const grandTotal = computed(() => {
  return bill.value + tipAmount.value;
});

const totalPerPerson = computed(() => {
  return grandTotal.value / people.value;
});

const selectPreset = (percent) => {
  tipPercent.value = percent;
  isCustom.value = false;
};

const handleCustomInput = () => {
  isCustom.value = true;
};

const handleBillInput = () => {
  if (bill.value === 0) {
    reset();
  }
};

const reset = () => {
  bill.value = 0;
  tipPercent.value = 10;
  people.value = 1;
  isCustom.value = false;
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap');

.font-poppins {
  font-family: 'Poppins', sans-serif;
}
</style>