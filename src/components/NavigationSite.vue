<template>
  <header class="header">
      <a href="/" class="flex-shrink-0">
        <h1 class="title ">Chopes mes notes</h1>
      </a>
      
      <!-- if logged  -->
      <div v-if="props.user.username.length > 0 " class="user-content">

            <div class="user__info">
                <i class="fa-solid fa-user"></i>
                <span class="name" >{{ props.user.username }}</span>
            </div>
        
            <div class="user__info logout !flex-row gap-2 cursor-pointer" @click="logout" >
                <span class="name">Sortir du Milano</span>
                <i class="fa-solid fa-arrow-right-from-bracket"></i>
            </div>
      </div>
      <!-- if not logged  -->
      <div v-else class="user-content">
        <!-- Loggin  -->
            <a class="user__info" :href="routes[2].url">
                <i class="fa-solid fa-user"></i>
                <span class="name" >{{ routes[2].name }}</span>
            </a>
          
        <!-- Register  -->
                 
            <a class="user__info" :href="routes[1].url">
                <i class="fa-solid fa-user-plus"></i>
                <span class="name" >{{ routes[1].name }}</span>
            </a>  
      </div>

      
  </header>
</template>

<script setup>
import { defineProps } from "vue";


const props = defineProps({
  user: Object
})


const routes = [
    {
        'name': "Accueil",
        'url': "/"
    },
    {
        'name': "Je s'appelle Comment ?",
        'url': "/jesappellecomment"
    },
    {
        'name': "Entrer dans le Milano",
        'url': "/entrerdansleMilano"
    }
]

function logout(){
    if (document.cookie['token'] != "" || document.cookie['username'] != "") {
        // this.$cookie.delete('token');
        // this.$cookie.delete('username');
        let date = new Date(Date.now() - 1000000);
        document.cookie = "token=" + "" + "; expires=" + date;
        document.cookie = "username=" + "" + "; expires=" + date;
        location.reload();
        // console.log(document.cookie)
    }
}
</script>

