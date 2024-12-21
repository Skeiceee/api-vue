<script setup>
  import Character from './components/Character.vue';
  import Pagination from './components/Pagination.vue';
  
  import axios from 'axios';
  import { onMounted, ref, watch } from 'vue';
  
  const characters = ref([]);
  const currentCharacter = ref([]);
  const page = ref(1);
  const pages = ref(1);
  const search = ref('');

  const isActiveModal = ref(false);
  const isLoadingModal = ref(false);

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

  const getDataOne = async (id) => {
    let result = await axios.get(`https://rickandmortyapi.com/api/character/${id}/`)
      .then((res) => {
        currentCharacter.value = res.data
        console.log(res.data)
      })
      .catch((err) => {
        console.log(err)
      })

    isLoadingModal.value = false
  }

  const showModal = (id) => {
    currentCharacter.value = []
    isLoadingModal.value = true
    getDataOne(id)
    isActiveModal.value = true
  }
  
  const closeModal = () => {
    isActiveModal.value = false
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
        <Character 
          v-for="character of characters"
          v-bind:key="character.id" 
          @showModal="showModal"
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

      <div class="modal" id="modal-js-example" :class="{ 'is-active' : isActiveModal }">
        <div class="modal-background"></div>
        <div class="modal-card" :class="{'skeleton-block' : isLoadingModal }">
            <header class="modal-card-head">
              <p class="modal-card-title" >{{ currentCharacter.name }}</p>
              <button class="delete" aria-label="close" @click="closeModal"></button>
            </header>
            <section class="modal-card-body">
              <div class="is-flex is-align-items-center is-justify-content-center">
                <figure class="image" :class="{ 'is-skeleton' : isLoadingModal, 'is-128x128' : isLoadingModal }"> 
                  <img class="is-rounded" :src="currentCharacter.image" alt="">
                </figure>
              </div>
              <div class="is-flex is-align-items-center is-justify-content-center mt-6 ">
                <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth" :class="{ 'is-skeleton' : isLoadingModal }">
                  <tbody>
                    <tr>
                      <td><strong>Genero</strong></td>
                      <td>{{ currentCharacter.gender }}</td>
                    </tr>
                    <tr>
                      <td><strong>Especie</strong></td>
                      <td>{{ currentCharacter.species }}</td>
                    </tr>
                    <tr>
                      <td><strong>Estado</strong></td>
                      <td>{{ currentCharacter.status }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </section>
            <footer class="modal-card-foot">
              <div class="buttons">
                  <button class="button" @click="closeModal">Cerrar</button>
              </div>
            </footer>
        </div>
      </div>

    </div>

  </div>
</template>