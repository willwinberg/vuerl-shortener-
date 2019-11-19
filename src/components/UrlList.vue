<template>
  <v-data-table
    :headers="headers"
    :items="urls"
    sort-by="name"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar color="orange">
        <v-toolbar-title>Generated URLs</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <UrlForm />
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline"></span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.name"
                      label="Name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.longUrl"
                      label="Long URL"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.shortUrl"
                      label="Short URL"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="red darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon small @click="deleteItem(item)">
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
import UrlForm from "./UrlForm.vue";
export default {
  name: "UrlInput",
  components: {
    UrlForm
  },
  data: () => ({
    dialog: false,
    headers: [
      {
        text: "Name",
        align: "left",
        value: "name"
      },
      { text: "Long URL", value: "longUrl" },
      { text: "Short URL", value: "shortUrl" },
      { text: 'Actions', value: 'action', sortable: false }
    ],
    urls: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      longUrl: "",
      shortUrl: ""
    },
    defaultItem: {
      name: "",
      longUrl: "",
      shortUrl: ""
    }
  }),
  computed: {},
  watch: {
    dialog(val) {
      val || this.close();
    }
  },
  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.urls = [
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
      ];
    },

    editItem(item) {
      this.editedIndex = this.urls.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.urls.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.urls.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.urls[this.editedIndex], this.editedItem);
      } else {
        this.urls.push(this.editedItem);
      }
      this.close();
    }
  }
};
</script>
