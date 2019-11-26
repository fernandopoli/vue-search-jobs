<template>
  <v-container grid-list-md mb-10>
    <v-layout mb-5 mt-5>
      <v-flex xs12>
        <h2 class="text-center">Please search Jobs that you need!</h2>
      </v-flex>
    </v-layout>
    <v-data-iterator
      :items="items"
      :items-per-page.sync="itemsPerPage"
      :page="page"
      :search="search"
      :sort-by="sortBy.toLowerCase()"
      :sort-desc="sortDesc"
      hide-default-footer
    >
      <template v-slot:header>
        <v-toolbar dark height="60" flat color="grey darken-4" class="mb-1">
          <v-text-field
            v-model="search"
            clearable
            flat
            solo-inverted
            hide-details
            prepend-inner-icon="search"
            placeholder="Search the Position"
          ></v-text-field>
          <template v-if="$vuetify.breakpoint.mdAndUp">
            <v-spacer></v-spacer>
            <v-select
              v-model="sortBy"
              flat
              solo-inverted
              hide-details
              :items="keys"
              prepend-inner-icon="arrow_downward"
              label="Sort by"
            ></v-select>
            <v-spacer></v-spacer>
            <v-btn-toggle v-model="sortDesc" mandatory>
              <v-btn depressed color="blue darken-2" :value="false">
                <v-icon>arrow_upward</v-icon>
              </v-btn>
              <v-btn depressed color="blue darken-2" :value="true">
                <v-icon>arrow_downward</v-icon>
              </v-btn>
            </v-btn-toggle>
          </template>
        </v-toolbar>
      </template>

      <template v-slot:default="props">
        <v-layout wrap grid-list-lg>
          <v-flex v-for="item in props.items" :key="item.company" xs12 sm6 md6 lg6>
            <v-card class="mx-auto pa-4" flat outlined elevation="8" tile height="100%">
              <v-card-text class="pa-1">
                <h3 class="primary--text text-uppercase text-center">{{ item.company }}</h3>
              </v-card-text>
              <v-list-item
                v-for="(key, index) in filteredKeys"
                :key="index"
                :color="sortBy === key ? `blue lighten-4` : `white`"
              >
                <v-list-item-content>
                  <v-list-item-subtitle>{{ item[key.toLowerCase()] }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-card>
          </v-flex>
        </v-layout>
      </template>

      <template v-slot:footer>
        <v-layout mt-2 wrap align-center justify-center>
          <span class="grey--text">Items per page</span>
          <v-menu offset-y>
            <template v-slot:activator="{ on }">
              <v-btn dark text color="primary" class="ml-2" v-on="on">
                {{ itemsPerPage }}
                <v-icon>keyboard_arrow_down</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item
                v-for="(number, index) in itemsPerPageArray"
                :key="index"
                @click="updateItemsPerPage(number)"
              >
                <v-list-item-title>{{ number }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>

          <v-spacer></v-spacer>

          <span class="mr-4 grey--text">Page {{ page }} of {{ numberOfPages }}</span>
          <v-btn depressed fab dark color="blue darken-3" class="mr-1" @click="formerPage">
            <v-icon>keyboard_arrow_left</v-icon>
          </v-btn>
          <v-btn depressed fab dark color="blue darken-3" class="ml-1" @click="nextPage">
            <v-icon>keyboard_arrow_right</v-icon>
          </v-btn>
        </v-layout>
      </template>
    </v-data-iterator>
  </v-container>
</template>

<style lang="scss" scoped>
@media (min-width: 1264px) {
  .flex.lg5-custom {
    width: 20%;
    max-width: 20%;
    flex-basis: 20%;
  }
}
</style>

<script>
import axios from "axios";

export default {
  data() {
    return {
      itemsPerPageArray: [6, 12, 100],
      search: "",
      filter: {},
      sortDesc: false,
      page: 1,
      itemsPerPage: 6,
      sortBy: "name",
      src: "",
      keys: ["Company", "requirements"],
      items: []
    };
  },
  mounted() {
    axios
      .get("/data/data.json")
      .then(response => {
        this.items = response.data;
      })
      .catch(error => {
        console.log(error);
      });
  },
  computed: {
    numberOfPages() {
      return Math.ceil(this.items.length / this.itemsPerPage);
    },
    filteredKeys() {
      return this.keys.filter(key => key !== `Company`);
    }
  },
  methods: {
    nextPage() {
      if (this.page + 1 <= this.numberOfPages) this.page += 1;
    },
    formerPage() {
      if (this.page - 1 >= 1) this.page -= 1;
    },
    updateItemsPerPage(number) {
      this.itemsPerPage = number;
    }
  }
};
</script>


