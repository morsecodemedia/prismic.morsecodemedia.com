<template>
  <div>
    <h1>
      Projects
    </h1>
    <div v-if="projects">
      <div
        v-for="p in projects.results"
        :key="p.uid"
      >
        <p>{{ p.data.project_title[0].text }}</p>
      </div>
    </div>
    <div v-else>
      <p>No results are available.</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Homepage',
  async asyncData ({ $prismic, error }) {
    try {
      // Query to get post content
      const projects = await $prismic.api.query([
        $prismic.predicates.at('document.type', 'case_study')
      ])
      // Returns data to be used in template
      return {
        projects
      }
    } catch (e) {
      // Returns error page
      error({ statusCode: 404, message: 'Page not found' })
    }
  },
  head () {
    return {
      title: '',
      meta: [
        { hid: 'ogtitle', property: 'og:title', content: '' },
        { hid: 'twtitle', name: 'twitter:title', content: '' },
        { hid: 'googlename', itemprop: 'name', content: '' },
        { hid: 'description', name: 'description', content: '' },
        { hid: 'ogdescription', property: 'og:description', content: '' },
        { hid: 'twdescription', name: 'twitter:description', content: '' },
        { hid: 'googledescription', itemprop: 'description', content: '' },
        { hid: 'ogurl', property: 'og:url', content: 'https://prismic.morsecodemedia.com' + this.$route.path },
        { hid: 'twsite', name: 'twitter:site', content: 'https://prismic.morsecodemedia.com' + this.$route.path }
      ],
      link: [
        { hid: 'canonical', rel: 'canonical', href: 'https://prismic.morsecodemedia.com' + this.$route.path }
      ]
    }
  }
}
</script>

<style>

</style>
