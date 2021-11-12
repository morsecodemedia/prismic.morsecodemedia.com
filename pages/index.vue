<template>
  <div>
    <h1>
      Hello World!
    </h1>
    <!-- <slices-block :slices="slices" /> -->
  </div>
</template>

<script>
// import SlicesBlock from '~/components/prismic/SlicesBlock.vue'

export default {
  name: 'Homepage',
  // components: {
  //  SlicesBlock
  // },
  async asyncData ({ $prismic, params, error }) {
    try {
      // Query to get post content
      const post = (await $prismic.api.query([
        $prismic.predicates.at('document.type', 'case_study')
      ])).data
      // Returns data to be used in template
      return {
        document: post
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
        { hid: 'ogurl', property: 'og:url', content: 'https://www.domain.com' + this.$route.path },
        { hid: 'twsite', name: 'twitter:site', content: 'https://www.domain.com' + this.$route.path }
      ],
      link: [
        { hid: 'canonical', rel: 'canonical', href: 'https://www.domain.com' + this.$route.path }
      ]
    }
  }
}
</script>

<style>

</style>
