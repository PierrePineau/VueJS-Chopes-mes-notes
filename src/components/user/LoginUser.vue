<template>
  <div class="login container">


    <h1 class="text-center">Le Milano va décoller !</h1>

    <form @submit.prevent="login" class="form">
      <div class="field__group ">
        <label class="field__label" for="username">Username</label>
        <input type="text" id="username" v-model="username" />
      </div>

      <div class="field__group">
        <label class="field__label" for="username">Mot de passe</label>
        <input type="password" id="password" v-model="password" />
      </div>
      

      
      <button class="btn">Entrer dans le Milano</button>
    </form>


    <a href="/jesappellecomment" class="mt-4">Je s'appelle comment ?</a>
  </div>
</template>

<script setup>
import { ref, $emit } from "vue";

const username = ref("");
const password = ref("");

const formdata = ref({
  username: username,
  password: password,
});


const login = async () => {
  const request = await fetch("https://projet.mds.ovh/user/login", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(formdata.value),
  });
  const response = await request.text();

  if (request.ok) {
    let date = new Date(Date.now() + 3600000);
    date = date.toUTCString();
    document.cookie = "token=" + response + "; expires=" + date;
    document.cookie = "username=" + username.value + "; expires=" + date;


    document.location.href = "/";
  } else if (request.status === 401) {
    console.error("error 401");
  } else if (request.status === 404) {
    console.error("error 404");
  } else {
    console.error("error");
  }
};
</script>

<style scoped lang="scss">
</style>
