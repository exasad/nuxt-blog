<template>
  <div>
    <div class="container mx-auto w-1/2 py-8">
      <Title>My Info | {{ title }}</Title>
    </div>
    <div>
      <div>Name: {{ user?.name }}</div>
      <div>Email: {{ user?.email }}</div>
    </div>
    <div class="mt-4">
      <h3 class="text-2xl">My Posts</h3>
      <ul v-if="posts">
        <li v-for="post in posts" :key="post.id">
          <NuxtLink
            class="text-blue-600 hover:underline"
            :to="`/posts/${post.id}`"
            >{{ post.title }}</NuxtLink
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
const title = useState('title')
const user = ref(null)
const posts = ref([])
const { $apiFetch } = useNuxtApp()
onMounted(async () => {
  const response = await $apiFetch('/api/user')
  user.value = response
  posts.value = await $apiFetch('/api/user/posts')
})
</script>