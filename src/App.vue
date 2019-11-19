<template>
  <v-app>
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col>
            <UrlList />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  import UrlList from './components/UrlList.vue'
export default {
  name: 'App',
  components: {
    UrlList
  },
  data: () => ({
    //
  })
};
</script>
