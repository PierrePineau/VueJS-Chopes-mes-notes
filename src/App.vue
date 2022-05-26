<template>
  <NavigationSite :user="user" />
  <component :is="view" :user="user"></component>
</template>

<script setup>
// Username Pierre = Pierro
// MDP Pierre = pierrepierro

// Username Caroline = Carolight
// MDP Caroline = Pimousse

// Rendu 24 mai
// https://github.com/rioukkevin/rioukkevin 
import { reactive, computed, ref , onMounted } from "vue";
import Home from "./components/NoteIndex.vue";
import Login from "./components/user/LoginUser.vue";
import Register from "./components/user/RegisterUser.vue";
import NavigationSite from "./components/NavigationSite.vue";

const routes = {
  "/": Home,
  "/jesappellecomment": Register,
  "/entrerdansleMilano": Login,
  // "/account": Account,
};
const path = ref(window.location.pathname);

const user = reactive({
  username: "",
  token: "",
});

const view = computed(() => {
  return routes[path.value || "/"] || Error;
});

onMounted(() => {
  const token_match = document.cookie.match(new RegExp("(^| )token=([^;]+)"));
  if (token_match) {
    user.token = token_match[2];
  }

  const user_match = document.cookie.match(new RegExp("(^| )username=([^;]+)"));
  if (user_match) {
    user.username = user_match[2];
  }
});

</script>


<style>

</style>
