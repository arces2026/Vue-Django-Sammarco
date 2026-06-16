<script setup>
import { ref } from 'vue';
let username = ref("");
let password = ref("");
let successMessage = ref("")


async function register(){
    const payload = {
        username: username.value,
        password: password.value
    }
    console.log('payload', payload)
    try {
        const response = await fetch("http://localhost:3000/api/register",{
            method: "POST",
            headers: {
                'Content-type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok){
            throw new Error(`Errore del server ${response.status}`);
        }

        const data = await response.json();

        if (data) {
            console.log('data', data)
            successMessage = `Utente ${username.value} registrato con successo.`
            username.value = '';
            password.value = '';
            return data
        }
            
    } catch (error) {
        console.error("Errore", error)
    }

    
}

</script>


<template>
    <form>
        <input type="text" placeholder="Username" v-model="username">
        <input type="password" placeholder="Password" v-model="password">
        <button @click.prevent="register">Register</button>
        <p>{{ successMessage }}</p>
    </form>
</template>