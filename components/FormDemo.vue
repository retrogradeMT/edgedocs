<template>
  <div class="ma-3">
    <v-card>

      <edge-form :headers="schema" :active="active" path="contacts" meta v-on:act="alert" />
    </v-card>
    <v-card class="mt-3">
      <v-card-text>
        <v-subheader>Form returns</v-subheader>
    <pre>{{active}}</pre>
        </v-card-text></v-card>

  </div>
</template>

<script>

export default {

  data () {
    return {
      active: {
        title: "",
        body: ""
      },
      schema: [
          {
            align: "left",
            field: "title",
            text: "",
            icon: "mdi-check-outline",
            protected: true,
            display: true,
            tableDisplay: true,
            filterDisplay: false,
            createDisplay: true,
            editable: true,
            sortable: true,
            label: "Note Title",
            type: "text",
            tableType: "text",
            required: true,
            custom_1: "",
            custom_2: "",
            custom_3: "",
            value: "title",
            used: "true",
            permission: "",
            flex: {
              xs: 12,
              sm: 12
            },
            order: 1,
            column: 1
          },
           {
            align: "left",
            field: "body",
            text: "",
            icon: "mdi-sign-text",
            protected: true,
            display: true,
            tableDisplay: false,
            filterDisplay: false,
            createDisplay: true,
            editable: true,
            sortable: true,
            label: "Note Details",
            type: "text",
            tableType: "text",
            required: false,
            custom_1: "",
            custom_2: "",
            custom_3: "",
            value: "body",
            used: "true",
            permission: "",
            flex: {
              xs: 12,
              sm: 12
            },
            order: 2,
            column: 2
          }

      ]
    }
  },
  methods: {
    alert (e) {
      alert(e)
    }
  }

}
</script>

<style lang="scss" scoped>

</style>
