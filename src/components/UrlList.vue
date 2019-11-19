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
        <v-dialog v-model="dialog" max-width="500px">
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
  </v-data-table>
</template>

<script>
export default {
  name: "UrlInput",
  components: {},
  props: {
    urls: Array
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
      { text: "Actions", value: "action", sortable: false }
    ],
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
  watch: {
    dialog(val) {
      val || this.close();
    }
  },
  methods: {
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
