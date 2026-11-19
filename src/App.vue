<script setup>
import { ref, computed } from 'vue'

const nouvelleTache = ref('')
const taches = ref([])
let prochainId = 1

function ajouter() {
  const libelle = nouvelleTache.value.trim()
  if (libelle === '') return
  taches.value.push({ id: prochainId++, libelle, terminee: false })
  nouvelleTache.value = ''
}

function supprimer(id) {
  taches.value = taches.value.filter(t => t.id !== id)
}

const nonTerminees = computed(
  () => taches.value.filter(t => !t.terminee).length
)
</script>

<template>
  <main class="app">
    <h1>Ma liste de tâches</h1>

    <div class="saisie">
      <input
        v-model="nouvelleTache"
        placeholder="Nom de la tâche"
        @keyup.enter="ajouter"
      />
      <button @click="ajouter">Ajouter</button>
    </div>

    <p v-if="taches.length === 0">Aucune tâche pour le moment.</p>

    <template v-else>
      <ul>
        <li v-for="tache in taches" :key="tache.id">
          <label :class="{ terminee: tache.terminee }">
            <input type="checkbox" v-model="tache.terminee" />
            {{ tache.libelle }}
          </label>
          <button @click="supprimer(tache.id)">Supprimer</button>
        </li>
      </ul>
      <p>Tâches non terminées : <strong>{{ nonTerminees }}</strong></p>
    </template>
  </main>
</template>

<style>
.app { max-width: 480px; margin: 40px auto; font-family: sans-serif; }
.saisie { display: flex; gap: 8px; margin-bottom: 16px; }
.saisie input { flex: 1; padding: 6px; }
ul { list-style: none; padding: 0; }
li { display: flex; justify-content: space-between; align-items: center;
     padding: 6px 0; border-bottom: 1px solid #ddd; }
.terminee { text-decoration: line-through; color: #888; }
</style>
