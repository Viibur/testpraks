<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted } from 'vue'
import { supabase } from './lib/supabaseClient'
import WelcomeItem from './components/WelcomeItem.vue';
import HelloWorld from './components/HelloWorld.vue';

let email = "";
let password = "";
let alert =  "";
let isLoggedIn = ref(false);
async function signupWithPassword() {

   const { data, error } = await supabase.auth.signInWithPassword({email, password});
   if (error) {
    console.log("Tekkis error: ", error)
   }
   if (data) {
    isLoggedIn.value = true
   }

   console.log(isLoggedIn)
}
  
</script>

<template>
 
  <div id="app">
    <div id="alert" v-if="alert">{{ alert }}</div>

    <form @submit.prevent="signupWithPassword">
      <label>
        Email address
        <input  v-model="email" />
      </label>
      <label>
        Password
        <input type="password" v-model="password" />
      </label>
      <button type="submit">Sign in</button>
    </form>
  </div>
  <div v-show="isLoggedIn">
  <HelloWorld />  
  </div>
  

</template>

<style scoped>
  button,
  input {
    display: block;
    margin-bottom: 10px;
  }

  #alert {
    color: red;
    margin-bottom: 10px;
  }
</style>