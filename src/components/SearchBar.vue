<script setup>
import { ref, watchEffect } from "vue";

const emit = defineEmits(["SearchByName"]);
const searchBusinessName = ref(null);

watchEffect(() => {
  if (searchBusinessName.value == "") {
    emit("SearchByName", null);
  }
});
</script>

<template>
  <div class="searchBar">
    <form>
      <input
        type="search"
        required
        v-model="searchBusinessName"
        @keyup.enter="$emit('SearchByName', searchBusinessName)"
      />
      <i class="fa fa-search" @click.prevent="$emit('SearchByName', searchBusinessName)">
        <svg
          width="24"
          height="24"
          xmlns="http://www.w3.org/2000/svg"
          fill-rule="evenodd"
          clip-rule="evenodd"
        >
          <path
            d="M15.853 16.56c-1.683 1.517-3.911 2.44-6.353 2.44-5.243 0-9.5-4.257-9.5-9.5s4.257-9.5 9.5-9.5 9.5 4.257 9.5 9.5c0 2.442-.923 4.67-2.44 6.353l7.44 7.44-.707.707-7.44-7.44zm-6.353-15.56c4.691 0 8.5 3.809 8.5 8.5s-3.809 8.5-8.5 8.5-8.5-3.809-8.5-8.5 3.809-8.5 8.5-8.5z"
          />
        </svg>
      </i>
      <a
        href="javascript:void(0)"
        id="clear-btn"
        @click.prevent="
          $emit('SearchByName', null), (searchBusinessName = null)
        "
        >Clear</a
      >
    </form>

  </div>
</template>

<style scoped>
.searchBar {
  margin-top: 50px;
  margin-bottom: 50px;
}


form {
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: all 1s;
  width: 50px;
  height: 50px;
  background: white;
  box-sizing: border-box;
  border-radius: 25px;
  border: 4px solid white;
  padding: 5px;
}

input {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 42.5px;
  line-height: 30px;
  outline: 0;
  border: 0;
  display: none;
  font-size: 1em;
  border-radius: 20px;
  padding: 0 20px;
}

.fa {
  box-sizing: border-box;
  padding: 10px;
  width: 42.5px;
  height: 42.5px;
  position: absolute;
  top: 0;
  right: 0;
  border-radius: 50%;
  color: #07051a;
  text-align: center;
  font-size: 1.2em;
  transition: all 1s;
}

form:hover,
form:valid {
  width: 250px;
  cursor: pointer;
}


form:hover input,
form:valid input {
  display: block;
}

form:hover .fa,
form:valid .fa {
  background: #07051a;
  color: white;
}
form:hover .fa svg path,
form:valid .fa svg path {
  fill: white;
}


a {
  display: none;
  position: absolute;
  top: 53px;
  bottom: 0;
  left: 0;
  right: 0;
  font-size: 20px;
  color: white;
  text-align: center;
  width: 100%;
}

form:valid a {
  display: block;
}
</style>
