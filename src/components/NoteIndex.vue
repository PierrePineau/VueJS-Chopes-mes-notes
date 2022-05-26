<template>
  <div class="container">

    <ListNote :user="user"/> 
    <!-- <AddNote @newnote="addnote"/>  -->
    <!-- <LoginUser />  -->
  </div>
</template>

<script setup>

  import ListNote from './notes/ListNote.vue'
  // import AddNote from './notes/AddNote.vue'
  import LoginUser from './user/LoginUser.vue'
  import { reactive, defineProps  } from "vue";

  const props = defineProps({
    user: Object,
  })

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
