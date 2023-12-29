<script setup>
defineProps({
  currentPage: {
    type: Number,
    required: true,
  },
  totalPages: {
    type: Number,
    required: true,
  },
  size: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="container">
    <div class="pagination">
      <ul>
        <li
          v-for="index in totalPages"
          :key="index"
          :class="index == currentPage ? 'active' : ''"
        >
          <a href="#" @click.prevent="$emit('changePage', index)"></a>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.container {
  padding: 1rem;
  margin: 3rem auto;
  border-radius: 0.2rem;
  box-shadow: 0 4px 6px 0 rgba(0, 0, 0, 0.3);
  counter-reset: pagination;
  text-align: center;
}
.container:after {
  clear: both;
  content: "";
  display: table;
}
.container ul {
  width: 100%;
}

ul,
li {
  list-style: none;
  display: inline;
  padding-left: 0px;
}

li {
  counter-increment: pagination;
}
li:hover a {
  color: #fdfdfd;
  background-color: #1d1f20;
  border: solid 1px #1d1f20;
}
li.active a {
  color: #fdfdfd;
  background-color: #4e5457;
  border: solid 1px white;
}
/* li:first-child {
  float: left;
}
li:first-child a:after {
  content: "Previous";
}
li:nth-child(2) {
  counter-reset: pagination;
}
li:last-child {
  float: right;
}
li:last-child a:after {
  content: "Next";
} */
li a {
  border: solid 1px #d7d7d7;
  border-radius: 0.2rem;
  color: #7d7d7d;
  text-decoration: none;
  text-transform: uppercase;
  display: inline-block;
  text-align: center;
  padding: 0.5rem 0.9rem;
}
li a:after {
  content: " " counter(pagination) " ";
}
</style>
