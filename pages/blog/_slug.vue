<template>
  <article class="page-container">
    <BlogPage
      v-if="blog"
      :read-time="`${blog.readTime}`"
      :created-at="$dateFns.format(blog.createdAt, 'PP')"
      :title="slug"
      :hero-pic="blog.heroImage.url"
      :description="blog.description"
      :html-string="blog.body"
    />
  </article>
</template>

<script>
import BlogPage from '../../components/blog/BlogPage.vue'
export default {
  components: { BlogPage },
  async asyncData({ params, $axios, context }) {
    const { slug } = params
    try {
      const { data } = await $axios.$get(`/api/Blog?titleName=${slug}`)

      return { blog: data?.items[0], slug }
    } catch (error) {
      console.log(error.response)
    }
  },
  data() {
    return {
      error: false,
    }
  },

  head() {
    return {
      title: this.slug,
    }
  },
}
</script>

<style lang="scss" scoped>
.page-container {
  max-width: 760px;
  margin: 0 auto;
  padding: 0 var(--horizontal-padding);
}
</style>
