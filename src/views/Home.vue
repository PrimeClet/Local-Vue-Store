<template>
  <div class="home">
    <div>
      <b-navbar toggleable="lg" type="dark" variant="success">
        <b-navbar-brand href="#">BridgeTask</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item href="/">Home</b-nav-item>
            <b-nav-item
              ><router-link to="/about">About</router-link></b-nav-item
            >
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-item-dropdown right v-if="login">
              <!-- Using 'button-content' slot -->
              <template #button-content>
                <b-avatar
                  variant="info"
                  src="https://placekitten.com/300/300"
                  class="mr-3"
                ></b-avatar>
                <span class="mr-auto">{{ userData[0].fullname }}</span>
              </template>
              <b-dropdown-item href="#" @click="supprimer"
                >Sign Out</b-dropdown-item
              >
            </b-nav-item-dropdown>
            <b-navbar-nav v-else>
              <b-nav-item
                ><router-link to="/login">Login</router-link></b-nav-item
              >
              <b-nav-item
                ><router-link to="/signin">SignIn</router-link></b-nav-item
              >
            </b-navbar-nav>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <b-row class="mt-4">
      <b-col md="10" offset-md="1">
        <b-row>
          <b-col cols="12" class="my-auto p-md-5 p-sm-2">
            <span class="float-left my-auto"
              ><h3 class="text-primary font-weight-bolder">
                Services List
              </h3></span
            >
            <span class="float-right">
              <b-button variant="outline-primary" class="float-left"
                ><router-link to="/services/create"
                  ><b-icon-plus-circle-fill class="mx-auto" /> Add
                  Service</router-link
                ></b-button
              >
            </span>
          </b-col>
        </b-row>
        <b-row class="mx-2">
          <b-col md="3" v-for="i in services" :key="i.id" class="mb-3 -mt-2">
            <b-card
              img-src="https://placekitten.com/1000/300"
              img-alt="Card image"
              img-top
            >
              <h4 class="font-weight-bolder">{{ i.Name }}</h4>
              <p>{{ i.Localisation }}</p>
              <p class="text-justify text-truncate">{{ i.Resume }}</p>

              <template #footer>
                <b-button
                  variant="outline-success"
                  class="float-left"
                  v-on:click="getItem(i.id)"
                  ><router-link :to="`/services/` + i.id"
                    >Voir</router-link
                  ></b-button
                >
                <b-button
                  variant="outline-primary"
                  class=""
                  v-on:click="getItem(i.id)"
                  ><router-link :to="`/services/modify/` + i.id"
                    >Edit</router-link
                  ></b-button
                >
                <b-button variant="outline-danger" class="float-right"
                  ><router-link :to="`/services/delete/` + i.id"
                    >Delete</router-link
                  ></b-button
                >
              </template>
            </b-card>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";

export default {
  name: "Home",
  data() {
    return {};
  },
  computed: {
    ...mapState(["login", "services", "userData"]),
    ...mapGetters({}),
    noMore() {
      return this.count >= this.totalPages - 1;
    },
    disabled() {
      return this.lloading || this.noMore;
    },
    player() {
      return this.$refs.surf[0].waveSurfer;
    },
    logged() {
      return this.$store.getters.loggedIn;
    }
  },
  methods: {
    getItem(toto) {
      this.$store.dispatch("getServiceV", toto).then(() => {
        this.$router.forward();
      });
    },
    supprimer() {
      this.$store.state.login = false;
    }
  }
};
</script>
