<template>
  <div class="container mx-auto w-1/2 py-8">
    <Title>{{ $route.params.id }} | {{ title}}</Title>
    <div class="text-3xl font-bold">
      <NuxtLink :to="`/posts/1`">This is post title</NuxtLink>
    </div>
    <div class="post-meta text-gray-700 flex items-center space-x-2">
      <div>feb 2,2022</div>
      <div>&middot;</div>
      <div>Asad Ali</div>
    </div>
    <div class="post-content mt-4">
      {{ $route.params.id }} is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing
    </div>
  </div>
</template>