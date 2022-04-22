<script>
import axios from 'axios'
export default {
  data() {
    return {
      email: "",
      password: "",
      count: 1
    }
  },
  methods: {
      async post() {
          const result = await axios("https://localhost:5001/identity/login", {
              data: {
                  userName: this.email,
                  password: this.password
              },
              method: "POST"
          })

          if(result.status === 200) {
              const token = result.data.token;
              window.localStorage.setItem("token", JSON.stringify(token))
              window.location.reload();
          }
      }
  },

  // `mounted` is a lifecycle hook which we will explain later
  mounted() {
    // `this` refers to the component instance.
    console.log(this.count) // => 1

    // data can be mutated as well
    this.count = 2
  }
}
</script>

<template>
    <div class="flex flex-col gap-8">
      <div class="flex flex-col gap-3">
        <div>
          <label
            htmlFor="email"
            className="block text-sm font-medium text-stone-300"
          >
            Email
          </label>
          <div className="mt-1">
            <input
              type="email"
              name="email"
              id="email"
              className="shadow-sm focus:ring-green-500 focus:border-green-500 block w-full sm:text-sm border-stone-700 bg-stone-800 rounded-md"
              placeholder="you@example.com"
              :value="email"
              @input="event => email = event.target.value"
            />
          </div>
        </div>
        <div>
          <label
            htmlFor="password"
            className="block text-sm font-medium text-stone-300"
          >
            Password
          </label>
          <div className="mt-1">
            <input
              type="password"
              name="password"
              id="password"
              className="shadow-sm focus:ring-green-500 focus:border-green-500 block w-full sm:text-sm border-stone-700 bg-stone-800 rounded-md"
              placeholder=""
              :value="password"
              @input="event => password = event.target.value"
            />
          </div>
        </div>
      </div>
      <button
        type="button"
        className="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
        @click="post"
      >
        Login
      </button>
    </div>
</template>