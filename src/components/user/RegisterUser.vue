<template>
  <div class="register container">


    <h1>Je s'appelle not groot ?</h1>

    <form @submit.prevent="register" class="form">
      <div class="field__group">
        <label for="name" class="field__label">Nom (et prénom)</label>
        <input type="text" id="name" v-model="name" />
      </div>
      <div class="field__group">
        <label for="username" class="field__label">Username</label>
        <input type="text" id="username" v-model="username" />
      </div>
      <div class="field__group">
        <label for="username" class="field__label">Mot de passe</label>
        <input type="password" id="password" v-model="password" />
      </div>
        
        <button class="btn">Je s'appelle {{username}}</button>
    </form>


    <a href="/entrerdansleMilano" class="mt-4">Entrer dans le Milano</a>
  </div>


</template>

<script setup>
import { ref } from "vue";

const name = ref("");
const username = ref("");
const password = ref("");


const formdata = ref({
  name: name,
  username: username,
  password: password,
});


const register = async () => {
  const request = await fetch("https://projet.mds.ovh/user/create", {
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