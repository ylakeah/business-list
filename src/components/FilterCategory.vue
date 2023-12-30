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

function toggle(params) {
  showFilter.value = !showFilter.value;
}
</script>

<template>
  <div class="filter-container centerized" @click.prevent="toggle()">
    Filter <span v-if="showFilter">&#9650;</span><span v-else>&#9660;</span>
  </div>

  <transition name="slide-fade">
    <div class="filter-options re-center list" v-if="showFilter">
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
          @click.prevent="$emit('filterCategory', selectedOptions), toggle()"
        >
          Apply
        </button>
        <button
          class="btm-btn"
          @click.prevent="
            $emit('filterCategory', []), (selectedOptions = []), toggle()
          "
        >
          Clear
        </button>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.centerized {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.re-center {
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
}
.filter-container {
  position: relative;
  cursor: pointer;
  padding: 10px;
  color: #fff;
  border-radius: 5px;
  display: inline-block;
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
  justify-content: center;
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
}

.filter-options label {
  display: block;
  margin: 5px 0;
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>
