<template>
  <div class="container mx-auto w-1/2 py-8">
    <!-- <div class="errors text-red-600 mb-4">Error Goes here</div> -->
    <ul v-if="errors.length > 0" class="errors text-red-600 mb-4">
      <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
    </ul>
    <h2 class="text-2xl">Edit Post</h2>
    <form action="#" class="space-y-6" @submit.prevent="updatePost">
      <div>
        <label for="title">Title</label>
        <input
          type="text"
          v-model="title"
          id="title"
          class="px-2 py-2 w-full shadow rounded mt-2"
        />
      </div>
      <div>
        <label for="body">Body</label>
        <textarea
          v-model="body"
          name="body"
          id="body"
          cols="30"
          rows="10"
          class="px-2 py-2 w-full shadow rounded mt-2"
        >
        </textarea>
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
          Update Post
        </button>
        <span v-show="isLoading">Loading.....</span>
      </div>
    </form>
    <div class="mt-4">
      <button
        @click="deletePost"
        onclick="return confirm('are u sure to delete?')"
      >
        DeletePost
      </button>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  middleware: ['auth'],
  // or middleware: 'auth'
})
const title = ref('')
const body = ref('')
const isLoading = ref(false)
const errors = ref([])
const router = useRouter()
const route = useRoute()
const { $apiFetch } = useNuxtApp()
const post = ref(null)

onMounted(async () => {
  try {
    post.value = await $apiFetch(`/api/postsAuth/${route.params.id}`)
    title.value = post.value.title
    body.value = post.value.body
  } catch (error) {
    window.location.pathname = '/'
  }
})

async function updatePost() {
  try {
    isLoading.value = true
    const post = await $apiFetch(`/api/post/${route.params.id}`, {
      method: 'PATCH',
      body: {
        title: title.value,
        body: body.value,
      },
    })
    title.value = ''
    body.value = ''
    alert('update post')
    isLoading.value = false
    router.push('/my-info')
  } catch (error) {
    if (error.response.status === 403) {
      alert(error.data.message)
    }
    isLoading.value = false
    errors.value = Object.values(error.data.errors).flat()
    console.log(error.data)
  }
}

async function deletePost() {
  try {
    isLoading.value = true
    const post = await $apiFetch(`/api/post/${route.params.id}`, {
      method: 'DELETE',
    })
    title.value = ''
    body.value = ''
    alert('deleted post')
    isLoading.value = false
    router.push('/my-info')
  } catch (error) {
    isLoading.value = false
    errors.value = Object.values(error.data.errors).flat()
    console.log(error.data)
  }
}
</script>