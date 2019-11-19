<template>
  <v-app>
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col>
            <UrlForm :urls="urls" />
            <UrlList :urls="urls" />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import UrlForm from "./components/UrlForm.vue";
import UrlList from "./components/UrlList.vue";
export default {
  name: "App",
  components: {
    UrlForm,
    UrlList
  },
  data: () => ({
    urls: [
      {
        name: "Red Wii",
        longUrl:
          "https://www.amazon.com/gp/product/B0046RDYPQ?pf_rd_p=183f5289-9dc0-416f-942e-e8f213ef368b&pf_rd_r=NGDXJMS8WYM0BNVE7K8B",
        shortUrl: "https://amzn.to/35bsVqO"
      },
      {
        name: "Eero",
        longUrl:
          "https://www.amazon.com/dp/B07YVVYDMJ/ref=ods_gw_ha_vicc_eero_3k?pf_rd_p=8c6a75a0-3d8c-4be3-8951-3c4e3351229e&pf_rd_r=KBYD08TWNMSMV0AS23MP&th=1",
        shortUrl: "https://amzn.to/2KATlKQ"
      }
    ]
  })
};
</script>
