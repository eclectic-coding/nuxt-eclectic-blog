<template>
  <article class="mx-auto p-10">
    <h1 class="text-4xl font-bold">{{ article.title }}</h1>
    <p class="mb-4 pl-4 text-gray-700">
      Posted on: {{ formatDate(article.createdAt) }}
    </p>
    <p>
      <span v-for="tag in article.tags" :key="tag">
        <span
          class="text-xs inline-flex items-center font-bold leading-sm uppercase px-3 py-1 bg-indigo-200 text-indigo-700 rounded-full mx-2 mb-2"
          >{{ tag }}</span
        >
      </span>
    </p>
    <nuxt-content class="mt-8" :document="article" />
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()

    return { article }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>

<style>
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}

.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}

.nuxt-content p {
  margin-bottom: 20px;
}

.nuxt-content a {
  font-weight: bold;
  @apply text-indigo-700;
}

.nuxt-content-highlight {
  margin: 20px;
}

.nuxt-content-highlight pre {
  border-radius: 10px;
}

p code {
  @apply bg-gray-200 py-1 px-2;

  border-radius: 5px;
}
</style>
