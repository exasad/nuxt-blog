<template>
  <div class="post-container py-8">
    <div class="text-3xl font-bold">
      <NuxtLink :to="`/posts/1`">This is post title</NuxtLink>
    </div>
    <div class="post-meta text-gray-700 flex items-center space-x-2">
      <div>feb 2,2022</div>
      <div>&middot;</div>
      <div>Asad Ali</div>
    </div>
    <div class="post-preview landing-relaxed mt-4">
      Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a
    </div>
    <div class="mt-4">
      <NuxtLink :to="`/posts/1`" class="bg-blue-700 hover:bg-blue-800 text-white rounded inline-block px-4 py-2">Read More</NuxtLink>
    </div>
  </div>
</template>