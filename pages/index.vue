<template >
  <div class="all">
    <v-layout column justify-center align-center>
      <v-flex xs12 sm8 class="wrapper">
        <v-card min-width="275" class="form">
          <v-snackbar v-model="snackbar" :timeout="4000" top>
            {{ message }}
            <v-btn dark flat @click="snackbar = false">Close</v-btn>
          </v-snackbar>

          <v-card-title>
            <h1 class="h1">Chat</h1>
          </v-card-title>
          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="name"
                :counter="16"
                :rules="nameRules"
                label="Your name"
                required
              ></v-text-field>

              <v-text-field v-model="room" :rules="roomRules" label="Enter the room" required></v-text-field>

              <v-btn :disabled="!valid" color="primary" class="mr-4" @click="submit">Log into</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  layout: "empty",
  head: {
    title: "Chat",
  },
  sockets: {
    connect: function () {},
  },
  data: () => ({
    valid: true,
    snackbar: false,
    message: "",
    name: "",
    nameRules: [(v) => !!v || "Enter your name"],
    room: "",
    roomRules: [(v) => !!v || "Enter your room"],
  }),

  mounted() {
    const { message } = this.$route.query;
    if (message === "noUser") {
      this.message = "Enter data";
    } else if (message === "leftChat") {
      this.message = "You left chat";
    }

    this.snackbar = !!this.message;
  },

  methods: {
    ...mapMutations(["setUser"]),
    submit() {
      if (this.$refs.form.validate()) {
        const user = {
          name: this.name,
          room: this.room,
        };

        this.$socket.emit("userJoined", user, (data) => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            user.id = data.userId;
            this.setUser(user);
            this.$router.push("/chat");
          }
        });
      }
    },
  },
};
</script>

<style>
.theme--dark.application {
  background-image: url(../images/1705.jpg);
  background-size: cover;
}

.v-card {
  background-color: rgb(18, 8, 68) !important;
}
</style>



