<script setup>
    import { defineEmits } from 'vue';

    const { image, name } = defineProps({
        id: Number,
        image: String,
        name: String,
    })

    const emit = defineEmits (['showModal']);
    
    const showMore = (id) => {
        console.log('entre')
        emit('showModal', id);
    }
</script>

<template>
    <div class="column is-12-mobile is-4-desktop is-4-tablet">
        <div class="card">
            <div class="card-header">
                <img class="image card-header-img" :src="image" :alt="name">
            </div>
            <div class="card-content">
                <h3 class="title is-size-4">{{ name }}</h3>
                <button class="button is-success is-small" @click="showMore(id)">Ver más...</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
  .card-header { 
    overflow: hidden; /* Asegura que la imagen no se salga del header */ 
  }

  .card-header-img { 
    width: 100%; height: 100%; object-fit: cover;
  }
</style>