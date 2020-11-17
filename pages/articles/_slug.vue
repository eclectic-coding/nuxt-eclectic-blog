<template>
  <div>
    <TheHeader />

    <article class="mx-auto w-4/5 p-10">
      <h1 class="text-4xl font-bold">{{ article.title }}</h1>
      <p class="mb-5 pl-4 text-gray-700">
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
  </div>
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

<style lang="scss">
.nuxt-content {
  h2 {
    @apply font-bold text-3xl;
  }

  h3 {
    @apply font-bold text-2xl;
  }

  p {
    @apply mb-5;
  }

  a {
    @apply text-indigo-700 font-bold;
  }

  ul {
    list-style: circle;
    padding-left: 2rem;
    margin-bottom: 1rem;
  }
}

.nuxt-content-highlight {
  @apply my-5;
}

.nuxt-content-highlight pre {
  @apply rounded-lg overflow-hidden;
}

p code {
  @apply bg-gray-200 py-1 px-2 rounded;
}
</style>
