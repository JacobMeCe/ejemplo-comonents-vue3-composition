<script setup>
import { ref, computed, onMounted } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpiner from "./components/LoadingSpinner.vue";

const posts = ref([]);
const postXPage = 10;
const inicio = ref(0);
const fin = ref(postXPage);
const loading = ref(true);

const favorito = ref("");

const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () => {
  inicio.value = inicio.value + postXPage;
  fin.value = fin.value + postXPage;
};

const prev = () => {
  inicio.value += -postXPage;
  fin.value += -postXPage;
};



const fetchData = async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log("error");
  } finally {
    loading.value = false;
  }
};

fetchData()

const maxLength = computed(() => posts.value.length);
</script>

<template>
  <LoadingSpiner v-if="loading" />
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mis Post Favorito: {{ favorito }}</h2>

    <PaginatePost
      @next="next"
      @prev="prev"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
      class="mb-2"
    />

    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      @cambiarFavorito="cambiarFavorito"
      class="mb-2"
    ></BlogPost>
  </div>
</template>

<style></style>


<!-- // onMounted(async () => {
//   // loading.value = true;
//   try {
//     const res = await fetch("https://jsonplaceholder.typicode.com/posts");
//     posts.value = await res.json()
//   } catch (error) {
//     console.log("error");
//   }finally{
//     loading.value = false
//   }
// });

// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then((res) => res.json())
//   .then((data) => (posts.value = data))
//   .catch((e) => console.log(e))
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false;
//     }, 2000);
//   }); -->