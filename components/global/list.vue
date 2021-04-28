<template>
  <v-list dense flat>
    <v-list-item v-for="(item, index) in items" :key="index">
      <v-list-item-icon>
            <v-icon> mdi-check-decagram </v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            {{item}}
          </v-list-item-content>

    </v-list-item>

  </v-list>
</template>

<script>
  export default {
    props: {
      items: {
        type: Array,
        required: true
      },
    },

  }
</script>

<style lang="scss" scoped>

</style>
