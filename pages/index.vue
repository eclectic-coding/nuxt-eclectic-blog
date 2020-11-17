<template>
  <main>
    <TheHeader />

    <div class="mx-auto">
      <ul class="flex flex-col justify-center items-center">
        <li
          v-for="article in articles.slice(0, 6)"
          :key="article.slug"
          class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12 article-card"
        >
          <NuxtLink
            :to="{ name: 'articles-slug', params: { slug: article.slug } }"
          >
            <img
              v-if="article.cover_image"
              :src="article.cover_image"
              alt="article.cover_image"
            />
            <h2 class="font-bold">{{ article.title }}</h2>
            <p class="text-sm">{{ formatDate(article.createdAt) }}</p>
            <p class="text-gray-600 text-sm">{{ article.description }}</p>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
import TheHeader from '@/components/TheHeader'
export default {
  components: { TheHeader },
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .only(['title', 'description', 'slug', 'cover_image', 'createdAt'])
      .sortBy('createdAt', 'desc')
      .fetch()
    return { articles }
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
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
