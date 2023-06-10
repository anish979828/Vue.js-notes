<template>
    <div>
        <p>
        <RouterLink class= "btn btn-outline-secondary" to="/">Go back to the list</RouterLink>
        </p> 
        <form @submit.prevent="handleSubmit">
        <h1>{{ model.id ? "Edit Post" : "Create New Post" }}</h1>
      
        <div class="mb-3">
            <input 
            type="text"

            v-model="model.title"
            class="form-control"
            placeholder="Post Title"
            >
        </div>
        <div class="mb-3">
            <input 
            v-model="model.body"

            type="textarea"
            class="form-control"
            placeholder="Post Body"
            >
        </div>
        <div class="mb-3">
            <button
            type="submit"
            :disabled="!model.title ||!model.title"
            class="btn btn-success">submit</button>
        </div>
    </form>
    </div>

   

</template>

<script setup>

import { ref,onMounted } from "vue";
import { useRoute,useRouter } from "vue-router";

const id = useRoute().params.id;
const router = useRouter();

const model = ref({
    id:"",
    title:"",
    body:""
});

onMounted(() => {
    if(!id){
        return;
    }
    fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
    .then((res) => res.json())
    .then((res) => {
        model.value = res;
    })
});

function handleSubmit(){
    if(model.value.id){
        fetch(`https://jsonplaceholder.typicode.com/posts/${model.value.id}`,{
            method:"PUT",
            body:JSON.stringify(model.value)
        })
        .then(res => res.json())
        .then(() => {
            router.push("/")
        }) 
    }
    else{
        fetch(`https://jsonplaceholder.typicode.com/posts`,{
            method:"POST",
            body:JSON.stringify(model.value)
        })
        .then(res => res.json())
        .then(() => {
            router.push("/")
        })
    }
}





</script>
<style>
</style>