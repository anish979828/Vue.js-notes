<template>
    <div>
        <h1>Article App</h1>
        <RouterLink to="/new" class="btn btn-success">Add new</RouterLink>
        <SinglePost v-for="post of postlist" :key="post.id" :post="post" @delete="onDelete" /> 
    </div>

</template>

<script setup>
import { ref,onMounted } from "vue";

import SinglePost from "../Components/SinglePost.vue";

const postlist = ref([]);


onMounted(() => {
    fetch(`https://jsonplaceholder.typicode.com/posts`)
    .then((res) => res.json())
    .then((res) => {
        postlist.value = res;
    })
})

function onDelete(post){
    postlist.value = postlist.value.filter(p => p.id !== post.id)
}

</script>

<style scoped>

</style>