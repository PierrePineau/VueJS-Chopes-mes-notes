<template>
<div class="container" v-if="props.user.username.length > 0 ">
  <!-- <span class="" @click="generatePoint">Générer des notes</span> -->
  <h2>Liste des notes de {{props.user.username}}</h2>
  <ul v-show="notes" class="note__list">
      <li class="note__item font-semibold mb-4">
        <span class="note__name">Nom</span>
        <span class="note__value">Note</span>
        <span class="note__value">Commentaire</span>
      </li>
      <li  v-for="note in notes" v-bind:key="note" class="note__item">
        <span class="note__name">{{ note.points.label }} :</span>
        <span class="note__value">{{ note.points.value }} </span>
        <span class="note__value">{{ note.points.comment }} </span>
      </li>
      <li v-show="moyenneNotes" class="mt-2 font-semibold note__moyenne">
        <span class="note__name">Moyenne : </span>
        <span class="note__value">{{ moyenneNotes}}</span>
      </li>
      <li v-show="moyenneClass" class=" font-light text-sm note__moyenne">
        <span class="note__name">Moyenne de la class : </span>
        <span class="note__value">{{ moyenneClass}}</span>
      </li>
  </ul>
  <button class="btn" @click="getNotes">Récupérer mes notes</button>
</div>
<div class="container" v-else>
  <h2>Entre dans le Milano pour obtenir vos notes !</h2>
</div>
  
</template>
    
<script setup>
import { defineProps, ref,  } from "vue";


const props = defineProps({
  user: Object,
});

const notes = ref("");
const totalNBnotes = ref(0);
const totalNotes = ref(0);
const moyenneNotes = ref(0);
const moyenneClass = ref(0);



const generatePoint = async () => {
  const request = await fetch("https://projet.mds.ovh/note/generate/" + props.user.username, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      "Authorization": props.user.token,
    },


  });
  const response = await request.text();

  if (request.ok) {
      console.log(response);
  }
  
};

const getNotes = async () => {
  const request = await fetch("https://projet.mds.ovh/note/" + props.user.username, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      "Authorization": props.user.token,
    },
  });
  const response = await request.json();

  if (request.ok) {
      // console.log(response);

      notes.value = response;
      // notes.value = notes.value.sort((a, b) => a.points.label < b.points.label );

      // notes.value = notes.value.sort(function (a, b) {
      //   return a.points.label - b.points.label;
      // });
      moyenne();
      // console.log('Notes récupéré');
      getMoyenneClass();
  }
};

const getMoyenneClass = async () => {
  const request = await fetch("https://projet.mds.ovh/note/class/average", {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      "Authorization": props.user.token,
    },
  });
  const response = await request.text();

  if (request.ok) {
      // console.log(response);

      moyenneClass.value = response;
  }
};

function moyenne(){
  totalNBnotes.value = 0;
  totalNotes.value = 0;
  notes.value.forEach(note => {
      totalNotes.value += note.points.value;
      totalNBnotes.value++;
  });
  moyenneNotes.value = totalNotes.value / totalNBnotes.value;
}

// onMounted(() => {
//   // getNotes();
// });




</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
