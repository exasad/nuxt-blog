<template>
  <div class="container mx-auto py-8" style="width: 20%">
    <ul v-if="errors.length > 0" class="errors text-red-600 mb-4">
      <li v-for="(error, index) in errors" :key="index">{{ error}}</li>
    </ul>
    <form action="#" class="space-y-6" @submit.prevent="login">
      <div>
        <label for="email">Email</label>
        <input
          type="text"
          v-model="email"
          id="email"
          class="px-2 py-2 w-full shadow rounded mt-2"
        />
      </div>
      <div>
        <label for="password">Email</label>
        <input
          type="password"
          v-model="password"
          id="password"
          class="px-2 py-2 w-full shadow rounded mt-2"
        />
      </div>
      <div>
        <button
          type="submit"
          class="
            inline-block
            bg-blue-600
            hover:bg-blue-700
            text-white
            rounded
            px-4
            py-2
          "
        >
          Login
        </button>
        <span v-show="isLoading">Loading.....</span>
      </div>
    </form>
  </div>
</template>

<script setup>
definePageMeta({
 middleware: ['guest']
})
const isLoading = ref(false)

const email = ref('')

const password = ref('')

const errors = ref([])

const { $apiFetch } = useNuxtApp()

function csrf() {
  return $apiFetch('/sanctum/csrf-cookie')
}
async function login() {
  isLoading.value = true
  await csrf()
  try {
    await $apiFetch('/login', {
      method: 'POST',
      body: {
        email: email.value,
        password: password.value,
      },
    })

// set data local storate
  const user = await $apiFetch('/api/user')

  const { setUser } = useAuth()

  setUser(user.name)

    window.location.href = '/my-info'
  } catch (error) {
    isLoading.value = false
    errors.value = Object.values(error.data.errors).flat()
    console.log(error.data)
  }
  isLoading.value = false
}
</script>