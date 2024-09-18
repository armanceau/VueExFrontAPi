<script setup>
import { ref, onMounted } from 'vue';

// const test = fetch ('http://localhost:3000/tasks').then(response => response.json()).then(data => console.log(data))

// console.log("test : " + await data)
// Variables réactives pour les tâches et les erreurs
const mesDonnees = ref([]);
const erreur = ref(null);

// Variables réactives pour le formulaire
const nom = ref('');
const description = ref('');
const _id = ref(''); // Assurez-vous que _id est bien généré ou fourni

// Fonction pour récupérer les tâches depuis l'API
const recupererDonnees = async () => {
  try {
    const response = await fetch('http://localhost:3000/tasks');
    if (!response.ok) {
      throw new Error('Erreur réseau');
    }
    mesDonnees.value = await response.json();
  } catch (error) {
    erreur.value = 'Erreur lors de la récupération des données : ' + error.message;
  }
};

// Fonction pour ajouter une tâche
const ajouterTache = async () => {
  try {
    const response = await fetch('http://localhost:3000/new-task', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        nom: nom.value,
        description: description.value,
        _id: _id.value,
      }),
    });

    if (!response.ok) {
      throw new Error('Erreur réseau');
    }

    const result = await response.json();
    console.log(result.message); // Affiche le message de succès

    // Réinitialiser les champs du formulaire
    nom.value = '';
    description.value = '';
    _id.value = '';

    // Mettre à jour la liste des tâches
    await recupererDonnees();
  } catch (error) {
    erreur.value = 'Erreur lors de l’ajout de la tâche : ' + error.message;
  }
};

// Appeler la fonction pour récupérer les tâches lorsque le composant est monté
onMounted(() => {
  recupererDonnees();
});
</script>

<template>
  <div class="card">
    <h2>Ajouter une tâche</h2>
    <form @submit.prevent="ajouterTache">
      <div>
        <label for="nom">Nom :</label>
        <input v-model="nom" type="text" id="nom" required />
      </div>
      <div>
        <label for="description">Description :</label>
        <input v-model="description" type="text" id="description" required />
      </div>
      <div>
        <label for="id">ID :</label>
        <input v-model="_id" type="text" id="id" required />
      </div>
      <button type="submit">Ajouter</button>
    </form>

    <!-- Affichage des erreurs s'il y en a -->
    <div v-if="erreur">
      <p>Erreur : {{ erreur }}</p>
    </div>

    <!-- Affichage des données récupérées -->
    <ul v-if="mesDonnees.length > 0">
      <li v-for="(task, index) in mesDonnees" :key="index">
        {{ task.nom }} | {{ task._id }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
