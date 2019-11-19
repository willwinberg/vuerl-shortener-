<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>
        <v-col cols="12" md="4">
          <v-text-field
            v-model="longUrl"
            :rules="urlRules"
            label="URL"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="4">
          <v-text-field
            v-model="name"
            :rules="nameRules"
            :counter="10"
            label="Name"
            required
          ></v-text-field>
        </v-col>
        <v-btn color="blue darken-1" @click="generateUrl">Generate</v-btn>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
const BITLY_URL = "https://api-ssl.bitly.com/v3/shorten?";
const BITLY_LOGIN = "o_4nm0b7lfsb";
const BITLY_KEY = "R_3acba2a6f39844c2adb685084688f6bd";

export default {
  name: "NewUrlForm",
  data: () => ({
    urls: [],
    valid: false,
    longUrl: "",
    shortUrl: "",
    name: "",
    urlRules: [v => !!v || "Url is required", v => v || "Url must be valid"],
    nameRules: [v => v.length <= 10 || "Name must be less than 10 characters"]
  }),
  methods: {
    // I was using too much time on this so I'm saving it for later
    checkUrl: url => {
      // const pattern = new RegExp(
      //   "^(https?:\\/\\/)?" + // protocol
      //   "((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|" + // domain name
      //   "((\\d{1,3}\\.){3}\\d{1,3}))" + // OR ip (v4) address
      //   "(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*" + // port and path
      //   "(\\?[;&a-z\\d%_.~+=-]*)?" + // query string
      //     "(\\#[-a-z\\d_]*)?$",
      //   "i"
      // ); // fragment locator
      // return !!pattern.test(url);
      return url;
    },
    addUrl() {
      this.urls.push({
        name: this.urls.name,
        url: this.longUrl,
        shortUrl: this.shortUrl
      });
      this.longUrl = "";
      this.shortUrl = "";
      this.name = "";
      // console.log(this.urls);
    },
    generateUrl() {
      let vm = this;

      this.axios
        .get(BITLY_URL, {
          params: {
            format: "json",
            apiKey: BITLY_KEY,
            login: BITLY_LOGIN,
            longUrl: this.longUrl
          }
        })
        .then(function(response) {
          if (response.status === 200) {
            vm.longUrl = response.data.data.long_url;
            vm.shortUrl = response.data.data.url;
            vm.addUrl();
          } else {
            console.log("Error generating URL");
          }
        })
        .catch(function(error) {
          console.log("Error: " + error);
        });
    }
  }
};
</script>
