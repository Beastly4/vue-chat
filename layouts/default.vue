<template>
  <v-app app dark>
    <v-navigation-drawer class="drawer-nav" app v-model="drawer" mobile-break-point="700px">
      <v-list subheader>
        <v-subheader class="subheader">People in the room</v-subheader>
        <hr />
        <v-list-tile v-for="u in users" :key="u.id" @click.prevent>
          <v-list-tile-content>
            <v-list-tile-title class="title-users">{{u.name}}</v-list-tile-title>
          </v-list-tile-content>

          <v-list-tile-action>
            <v-icon v-if="u.id === user.id" color="primary">star</v-icon>
            <v-icon v-else color="primary">star_border</v-icon>
          </v-list-tile-action>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app class="toolbar">
      <v-toolbar-side-icon @click="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn icon @click="exit">
        <v-icon>arrow_back</v-icon>
      </v-btn>
      <v-toolbar-title class="toobar-title">
        Chat room
        <div class="room-num">{{user.room}}</div>
      </v-toolbar-title>
    </v-toolbar>
    <v-content>
      <div class="main-content">
        <nuxt />
      </div>
    </v-content>
  </v-app>
</template>

<script>
import { mapState, mapMutations } from "vuex";
export default {
  data: () => ({
    drawer: true,
  }),
  computed: mapState(["user", "users"]),
  methods: {
    ...mapMutations(["clearData"]),
    exit() {
      this.$socket.emit("userLeft", this.user.id, () => {
        this.$router.push("/?message=leftChat");
        this.clearData();
      });
    },
  },
};
</script>

<style scoped>
.main-content {
  height: 100%;
}

.toolbar {
  background-color: #630e51;
}

.drawer-nav {
  background-color: #630e51;
}

.toobar-title {
  display: flex;
  justify-content: center;
}

.title-users {
  font-size: 17px;
  font-family: "Dance", cursive;
}

.room-num {
  padding: 0 0 0 7px;
  font-family: "Dance", cursive;
}

.subheader {
  font-size: 17px;
  display: flex;
  justify-content: center;
  font-family: "Ranchers", cursive;
  font-weight: 700;
  color: white;
}

@font-face {
  font-family: "Ranchers";
  src: url("~@/fonts/Ranchers-Regular.ttf") format("ttf");
}

@font-face {
  font-family: "Dance";
  src: url("~@/fonts/DancingScript-VariableFont_wght.ttf") format("ttf");
}
</style>
