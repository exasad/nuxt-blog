<template>
  <div class="container mx-auto py-8" style="width: 20%">
    <!-- <div class="errors text-red-600 mb-4">Error Goes here</div> -->
    <ul v-if="errors.length > 0" class="errors text-red-600 mb-4">
      <li v-for="(error, index) in errors" :key="index">{{ error}}</li>
    </ul>
    <form action="#" class="space-y-6" @submit.prevent="register">
    <div>
        <label for="name">Name</label>
        <input
          type="text"
          v-model="name"
          id="name"
          class="px-2 py-2 w-full shadow rounded mt-2"
        />
      </div>
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
        <label for="password">Password</label>
        <input
          type="password"
          v-model="password"
          id="password"
          class="px-2 py-2 w-full shadow rounded mt-2"
        />
      </div>
      <div>
        <label for="password_confirmation">Confirm Password</label>
        <input
          type="password"
          v-model="password_confirmation"
          id="password_confirmation"
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
          Register
        </button>
        <span v-show="isLoading">Loading.....</span>
      </div>
    </form>
  </div>
</template>

<script setup>

definePageMeta({
  middleware: ['guest']
  // or middleware: 'auth'
})


const isLoading = ref(false)

const name = ref('')

const email = ref('')

const password = ref('')

const password_confirmation = ref('')

const errors = ref([])

const { $apiFetch } = useNuxtApp()

function csrf() {
  return $apiFetch('/sanctum/csrf-cookie')
}
async function register() {
  isLoading.value = true
  await csrf()
  try {
   await $apiFetch('/register', {
      method: 'POST',
      body: {
        name: name.value,
        email: email.value,
        password: password.value,
        password_confirmation: password_confirmation.value
      },
    })

    const user = await $apiFetch('/api/user')
    const { setUser } = useAuth()
    setUser(user.name)
    alert('Successfully registered')
    window.location.href = '/my-info'
  } catch (error) {
    isLoading.value = false
    errors.value = Object.values(error.data.errors).flat()
    console.log(error.data)
  }
  isLoading.value = false
}
</script>