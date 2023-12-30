<script setup>
import { ref, onMounted, watchEffect, reactive } from "vue";
import Pagination from "./Pagination.vue";
import SearchBar from "./SearchBar.vue";
import BusinessCard from "./BusinessCard.vue";
import FilterCategory from "./FilterCategory.vue";

defineProps({
  msg: {
    type: String,
    required: true,
  },
});

const bizhareAPI = import.meta.env.VITE_BIZHARE_API;
const businessList = ref(null);
const loading = ref(true);
const error = ref(null);
const categoryList = ref([]);

const paramsBody = reactive({
  businessName: null,
  size: 10,
  page: 1,
  listCategory: [],
});

function fetchData() {
  loading.value = true;
  return fetch(`${bizhareAPI}/business/parent/all`, {
    method: "POST",
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(paramsBody),
  })
    .then((res) => {
      // a non-200 response code
      if (!res.ok) {
        // create error instance with HTTP status text
        const error = new Error(res.statusText);
        error.json = res.json();
        throw error;
      }

      return res.json();
    })
    .then((json) => {
      // set the response data
      businessList.value = json.data;
    })
    .catch((err) => {
      error.value = err;
      // In case a custom JSON error response was provided
      if (err.json) {
        return err.json.then((json) => {
          // set the JSON response message
          error.value.message = json.message;
        });
      }
    })
    .then(() => {
      loading.value = false;
    });
}

function fetchCategory() {
  loading.value = true;
  return fetch(`${bizhareAPI}/media/param/business/category`, {
    method: "GET",
    headers: {
      "content-type": "application/json",
    },
  })
    .then((res) => {
      // a non-200 response code
      if (!res.ok) {
        // create error instance with HTTP status text
        const error = new Error(res.statusText);
        error.json = res.json();
        throw error;
      }

      return res.json();
    })
    .then((json) => {
      // set the response data
      categoryList.value = json.data;
    })
    .catch((err) => {
      error.value = err;
      // In case a custom JSON error response was provided
      if (err.json) {
        return err.json.then((json) => {
          // set the JSON response message
          error.value.message = json.message;
        });
      }
    })
    .then(() => {
      loading.value = false;
    });
}

onMounted(() => {
  fetchData();
  fetchCategory();
});
</script>

<template>
  <div class="greetings">
    <div>
      <SearchBar
        @search-by-name="
          (name) => {
            paramsBody.businessName = name;
            fetchData();
          }
        "
      />
    </div>

    <div>
      <FilterCategory
        :categoryList="categoryList"
        @filter-category="
          (list) => {
            paramsBody.listCategory = list;
            paramsBody.page = 1;
            fetchData();
          }
        "
      />
    </div>

    <div class="flex-container" v-if="!loading">
      <BusinessCard
        :currentPage="paramsBody.page"
        :index="index + 1"
        v-for="(item, index) in businessList?.content"
        :key="item?.id"
        :business="item"
      />
    </div>
    <div v-else>
      Loading..
    </div>

    <div>
      <Pagination
        :totalPages="businessList?.totalPages || 0"
        :size="businessList?.size || 0"
        :currentPage="paramsBody.page"
        @change-page="
          (page) => {
            paramsBody.page = page;
            fetchData();
          }
        "
      />
    </div>
  </div>
</template>

<style scoped>
.flex-container {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-content: space-around;
}

.flex-container > div {
  color: white;
  width: 100px;
  margin: 10px;
  flex: 0 1 25%;
  text-align: center;
  line-height: 75px;
  font-size: 30px;
}

@media (max-width: 800px) {
  .flex-container > div {
    max-width: 75%;
    flex: 0 1 30%;
  }
}

/* Responsive layout - makes the two columns stack on top of each other instead of next to each other */
@media (max-width: 600px) {
  .flex-container > div {
    max-width: 100%;
    flex: 0 1 100%;
  }
}
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

.greetings {
  /* max-width: 1024px; */
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
