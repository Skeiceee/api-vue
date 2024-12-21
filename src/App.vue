<script setup>
  import Character from './components/Character.vue';
  import Pagination from './components/Pagination.vue';
  
  import axios from 'axios';
  import { onMounted, ref, watch } from 'vue';
  
  const characters = ref([]);
  const page = ref(1);
  const pages = ref(1);
  const search = ref('');

  const startPage = () => {
    page.value = 1;
  }

  const endPage = () => {
    page.value = pages.value;
  }

  const nextPage = () => {
    if(page.value === pages.value) return;
    page.value += 1
  }

  const prevPage = () => {
    if(page.value <= 1) return;
    page.value -= 1
  }

  const getData = () => {
    const params = {
      page: page.value,
      name: search.value
    }

    let result = axios.get('https://rickandmortyapi.com/api/character', { params })
      .then((res) => {
        characters.value = res.data.results
        pages.value = res.data.info.pages
        console.log(res.data)
      })
      .catch((err) => {
        console.log(err)
      })
  }

  const searchData = () => {
    page.value = 1
    getData()
  }

  watch(
    page, () => { getData(); }
  );

  onMounted(() => { 
    getData(); 
  });

</script>

<template>
  <div id="app">

    <div class="hero is-white is-gradient is-bold">

      <div class="hero-body">

        <h1 class="title">
          <span class="has-text-success mr-4">Rick and Morty</span>
          <span class="subtitle">Personajes</span>
        </h1>

        <div class="field has-addons is-pulled-right" >
          <div class="control">
            <input 
              class="input no-borde-derecho" 
              type="text"
              v-model="search" 
              @keyup.enter="searchData">
          </div>
          <div class="control">
            <button 
              class="button is-success no-borde-izquierdo" 
              @click="searchData"
            >Buscar</button>
          </div>
        </div>

        <!-- <button class="button is-success" @click="fetchData">Consultar</button> -->

      </div>

    </div>

    <div class="container mt-6">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered p-4">
        <Character v-for="character of characters" 
          v-bind:key="character.id" 
          :id="character.id" 
          :image="character.image" 
          :name="character.name">
        </Character>
      </div>

      <div class="container p-4">

        <Pagination 
          @startPage="startPage" 
          @prevPage="prevPage" 
          @nextPage="nextPage" 
          @endPage="endPage"
          :page="page" 
          :pages="pages">
        </Pagination>
        
      </div>
    </div>

  </div>
</template>