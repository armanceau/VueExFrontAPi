<script setup>
import { ref, onMounted } from 'vue'

defineProps({
  name: String,
  description: String
});

const mesDonnees = ref(null);
const erreur = ref(null);

const recupererDonnees = async () => {
  try {
    const response = await fetch('http://localhost:3000/tasks');  // Remplace par l'URL de ton API
    if (!response.ok) {
      throw new Error('Erreur réseau');
    }
    const data = await response.json();  // Attendre la résolution de la promesse
    mesDonnees.value = data;
    // console.log("data 1 :", data[0]._id);
    // console.log("data tot. :", data);
    // console.log("mes données :", mesDonnees.value[0]._id);
  } catch (error) {
    erreur.value = 'Erreur lors de la récupération des données : ' + error.message;
  }
};

// Appeler la fonction lorsque le composant est monté
onMounted(() => {
  recupererDonnees();
});
</script>

<template>

  <div class="card">
    <ul v-if="mesDonnees">
      <li v-for="(task, index) in mesDonnees" :key="index">
        {{ task._id }} | {{ task.nom }}
      </li>
    </ul>
  </div>

</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
