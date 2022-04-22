<script setup>
import LoginComponent from "./components/LoginComponent.vue";
</script>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      user: null
    }
  },
  async mounted() {
    const token = window.localStorage.getItem("token")
    if(!token) return;
    const result = await axios("https://localhost:5001/identity/user",
      {
        headers: {
          "Authorization": `Bearer ${JSON.parse(token)}`
        }
    })
    console.log(result.data)
    if(result.status === 200) {
      this.user = result.data;
    }
  },
  methods: {
    logout() {
      window.localStorage.removeItem("token");
      window.location.reload();
    }
  }
}
</script>


<template>
  <div v-if="user === null" class="flex flex-col md:flex-row gap-10 items-center">
    <div>
      <h1 class="text-2xl">Authentication in vue</h1>
    </div>
    <login-component />
  </div>
  <div v-if="user !== null" class="flex flex-col gap-y-4">
    <h2 class="text-3xl text-center">Logged in as <b>{{user.userName}}</b></h2>
    <div class="flex justify-center">
      <button
        type="button"
        className="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md shadow-sm text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
        @click="logout"
      >
        Logout
      </button>
    </div>
    <details>
      <summary>See user information</summary>
      <div class="flex flex-col bg-stone-700 text-stone-200 p-5 py-10 rounded-lg shadow-md">
        <div 
          class="flex justify-between gap-x-8"
          v-for="(value, key) in user"
          :key="value"
        >
          <b>{{ key }}: </b>
          <p>{{ value }}</p>
        </div>
      </div>
    </details>
  </div>
  <div class="fixed bottom-14 w-full flex justify-center left-0">
    <p>Made with <span class="text-red-500 font-bold">love</span> by <a href="https://github.com/notlimey" target="_blank" rel="noreferrer">Limey</a></p>
  </div>
</template>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

details {
  transition: all .5s;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
