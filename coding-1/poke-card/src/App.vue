<script setup>
import { ref } from 'vue'

const search = ref('')

</script>

<template>
  <header>
    <div class="logo-box">
        <div class="logo-image">P</div>
        <div class="logo-text">Poke Card</div>
    </div>

    <nav>
      <input type="text" v-model="search">
      <button  @click="getPoke(search)">Buscar</button>
    </nav>
  </header>

  <br />

  <div v-if="pokeData != null" class="container">
    <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ pokeData.id }.png`" width="200" />

    <div>
      Type:
      <template v-for="(type, index) in pokeData.types">
        <template v-if="index > 0">, </template>
        <span>{{ type.type.name }}</span>
      </template>
    </div>

    <ul>
      <li v-for="(stat, index) in pokeData.stats" v-bind:key={index}>
        {{ stat.stat.name }} - {{ stat.base_stat }}
      </li>
    </ul>
  </div>

  <h1 v-if="!pokeData && notFound">Oh no 😢, Pokemon doesn't exist</h1>

  <!-- <RouterView /> -->
</template>

<script>

export default {
  data() {
    return {
      pokeData: null,
      notFound: false
    }
  },
  methods: {
    getPoke(value) {
      fetch('https://pokeapi.co/api/v2/pokemon/' + value.toLowerCase())
        .then(response => response.json())
        .then(data => this.pokeData = data)
        .catch(error => {
          this.pokeData = null;
          this.notFound = true;
        })
    }
  }
}
</script>

<style scoped>
header {
  display: flex;
  width: 100%;
}

.logo-box {
  display: flex;
  align-items: center;
}

.logo-image {
  display: flex;
  padding: 10px 20px;
  font-size: 3rem;
  background-color: #1E88E5;
  color: #ffffff;
  margin-right: 10px;
}

.logo-text {
  color: #1E88E5;
}

nav {
  width: 100%;
  text-align: center;
  margin-top: 2rem;
}

#search {
  width: 300px;
  padding: 8px 15px;
  border-radius: 5px;
  font-size: 1rem;
  outline: none;
  border-width: thin;
}

.container {
  width: 100%;
  margin-top: 30px;
  border: 1px solid grey;
}
</style>
