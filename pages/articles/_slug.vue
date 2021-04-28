<template>
  <v-app id="edge">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-avatar
        :color="$vuetify.breakpoint.smAndDown ? 'grey darken-1' : 'transparent'"
        size="32"
      ></v-avatar>

      <v-tabs
        centered
        class="ml-n9"
        color="grey darken-1"
      >
        <v-tab
          v-for="(link, index) in articles"
          :key="index"
        >
          {{ link.title }}
        </v-tab>
      </v-tabs>

      <v-avatar
        class="hidden-sm-and-down"
        color="grey darken-1 shrink"
        size="32"
      ></v-avatar>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <v-container fluid>
        <v-row>
          <v-col
            cols="12"
            md="2"
          >
            <v-card
              rounded="lg"
              min-height="268"
              flat
            >
              <v-list>
                  <v-list-item v-for="(link, index) in articles" :key="index">
                    <v-list-item-action>
                      <v-icon light>
                        mdi-arrow-right-thin-circle-outline
                      </v-icon>
                    </v-list-item-action>
                    <v-list-item-title>{{link.title}}</v-list-item-title>
                  </v-list-item>
                </v-list>
            </v-card>
          </v-col>

          <v-col
            cols="12"
            md="8"
          >
            <v-card
              min-height="70vh"
              rounded="lg"
            ><v-card-text>


            <article>
              <div class="overline">{{ article.description }}</div>
              <h1>{{ article.title }}</h1>
            <div class="primary--text text--darken-2 px-1 my-2">Written by: {{ article.author }}</div>
            <div class="primary--text text--darken-2 px-1 my-2">Updated at: {{ formatDate(article.updatedAt) }}</div>
    <nuxt-content :document="article" class="absolute;
    grey--text text--darken-2 mr-2 mt-1" />
  </article>
  </v-card-text>

            </v-card>
          </v-col>

          <v-col
            cols="12"
            md="2"
          >
            <v-sheet
              rounded="lg"
              min-height="268"

            >
            <v-card-title>On this page</v-card-title>

              <v-list nav dense>
                 <v-list-item-group
          v-model="selectedContent"
          color="primary"
        >
                <v-list-item v-for="link of article.toc" :key="link.id" link>
                  <v-list-item-content>
                        <v-list-item-title><NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink></v-list-item-title>
                      </v-list-item-content>
                </v-list-item>
                </v-list-item-group>
              </v-list>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  async asyncData ({ $content, params }) {
    const article = await $content("articles", params.slug).fetch()

    const articles = await $content('articles').only(['title', 'path']).sortBy('title').fetch()
return { article, articles }

  },
   data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      selectedContent: 0,
      title: 'Edge marketing Vue Packages'
    }
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
h1 {
    font-weight: bold;
    font-size: 32px;
  }
  .nuxt-content h2 {
    font-weight: bold;
    font-size: 28px;
    margin-bottom: .75rem;
    margin-top: 2.5rem;
    border-bottom: 1px solid #e1e1e1;
  }
  .nuxt-content h3 {
    font-weight: bold;
    font-size: 22px;
     margin-bottom: .75rem;
    margin-top: 2.5rem;
  }
  .nuxt-content h4 {
    font-weight: bold;
    font-size: 18px;
    margin-bottom: .5rem;
    margin-top: 2rem;
  }
  .nuxt-content p {
    margin-bottom: 20px;
  }
  .nuxt-content-highlight {
  position: relative;
}

  table {
  border-collapse: collapse;
}

td, th {
  border: 1px solid #999;
  padding: 0.5rem;
  text-align: left;
}
</style>
