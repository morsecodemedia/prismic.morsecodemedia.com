<template>
  <div class="page-projects">
    <h1>
      Projects
    </h1>
    <div v-if="projects" class="project-list">
      <div
        v-for="p in projects.results"
        :key="p.uid"
        class="project-card"
      >
        <h2>{{ p.data.project_title[0].text }}</h2>
        <p>{{ p.data.client }}</p>
        <p>{{ p.data.project_category }}</p>
        <p>
          <span>{{ p.data.year_start[0].text }}</span> -
          <span v-if="p.data.year_end[0]">{{ p.data.year_end[0].text }}</span>
          <span v-else>Present Day</span>
        </p>
      </div>
      <nuxt-link
        v-if="notFirstPage"
        :to="prevPage"
      >
        &laquo; Previous Page
      </nuxt-link>
      <nuxt-link
        :to="nextPage"
      >
        Next Page &raquo;
      </nuxt-link>
    </div>
    <div v-else>
      <p>No results are available.</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Homepage',
  async asyncData ({ $prismic, params, error }) {
    try {
      const projects = await $prismic.api.query(
        [
          $prismic.predicates.at('document.type', 'case_study')
        ],
        {
          pageSize: 100,
          page: params.num,
          orderings: '[my.case_study.year_start desc, my.case_study.year_end desc, my.case_study.client, my.case_study.project_title]'
        }
      )
      return {
        projects
      }
    } catch (e) {
      console.log(e)
    }
  },
  computed: {
    notFirstPage () {
      return this.$route.params.num > 1
    },
    prevPage () {
      if (this.$route.params.num === '2') {
        return '/'
      }
      return '/page/' + (this.$route.params.num - 1)
    },
    nextPage () {
      return '/page/' + (this.$route.params.num + 1)
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
.page-projects {
  max-width: 1200px;
  margin: 0 auto;
  padding: 15px;
}
.project-list {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.project-card {
  border: 1px solid black;
  border-radius: 15px;
  padding: 10px;
  margin: 12px;
  width: 350px;
}
</style>
