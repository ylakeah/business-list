<script setup>
import { ref, watchEffect } from "vue";
defineProps({
  categoryList: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["filterCategory"]);
const showFilter = ref(false);
const selectedOptions = ref([]);
</script>

<template>
  <div class="filter-container" @click.prevent="showFilter = !showFilter">
    Filter <span v-if="showFilter">&#9650;</span><span v-else>&#9660;</span>
  </div>

  <div v-if="showFilter" class="filter-options">
    <label v-for="(option, index) in categoryList" :key="index">
      <input
        type="checkbox"
        v-model="selectedOptions"
        :value="option.paramCode"
      />
      {{ option.paramName }}
    </label>

    <div class="btm-section">
      <button
        class="btm-btn"
        @click.prevent="$emit('filterCategory', selectedOptions)"
      >
        Apply
      </button>
      <button
        class="btm-btn"
        @click.prevent="$emit('filterCategory', []), (selectedOptions = [])"
      >
        Clear
      </button>
    </div>
  </div>
</template>

<style scoped>
body {
  font-family: "Arial", sans-serif;
  text-align: center;
  margin: 20px;
}

.filter-container {
  cursor: pointer;
  padding: 10px;
  color: #fff;
  border-radius: 5px;
  display: inline-block;
  transition: 0.3s;
  border: silver 0.5px solid;
}

.btm-section {
  margin: 15px;
}
.btm-btn {
  margin-right: 15px;
  cursor: pointer;
  padding: 10px;
  background-color: black;
  color: #fff;
  border-radius: 5px;
  /* display: flex; */
  justify-content: center;
  transition: 0.3s;
  border: silver 0.5px solid;
}

.btm-btn:hover {
  background-color: white;
  color: black;
}

.filter-container:hover {
  background-color: white;
  color: black;
}

.filter-options {
  position: absolute;
  background-color: black;
  z-index: 9;
  width: 20rem;
  padding: 10px 24px;
  border-radius: 15px;
  border: silver 0.7px solid;
  margin-top: 10px;
  overflow: hidden;
  transition: max-height 0.3s ease-out;
}

.filter-options label {
  display: block;
  margin: 5px 0;
}
</style>
