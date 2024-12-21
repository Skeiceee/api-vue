<script setup>
    import { defineEmits } from 'vue';

    const { page, pages } = defineProps({
        page: Number,
        pages: Number,
    })
    
    const emit = defineEmits(['startPage', 'prevPage', 'nextPage', 'endPage']);
    const startPage = () => { emit('startPage'); }
    const prevPage = () => { emit('prevPage'); }
    const nextPage = () => { emit('nextPage'); }
    const endPage = () => { emit('endPage'); }

</script>
<template>
    <nav class="pagination is-right" role="navigation">
        <a @click="prevPage" class="pagination-previous">Previous</a>
        <a @click="nextPage" class="pagination-next">Next page</a>
        <ul class="pagination-list">
        <li v-if="page > 1">
            <a @click="startPage" class="pagination-link">1</a>
        </li>

        <li v-if="page > 1"><span class="pagination-ellipsis">&hellip;</span></li>
        
        <li v-if="page > 1">
            <a @click="prevPage" class="pagination-link">{{ page - 1 }}</a>
        </li>

        <li>
            <a
            class="pagination-link is-current"
            >{{ page }}</a
            >
        </li>

        <li v-if="page !== pages">
            <a @click="nextPage" class="pagination-link">{{ page + 1 }}</a>
        </li>

        <li v-if="page !== pages"><span class="pagination-ellipsis">&hellip;</span></li>

        <li v-if="page !== pages">
            <a @click="endPage" class="pagination-link">{{ pages }}</a>
        </li>
        </ul>
    </nav>
</template>