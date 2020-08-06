<template>
  <v-col xs12>
    <div class="form-wrapper">
      <v-text-field
        class="form"
        label="Enter message"
        outline
        dense
        v-model="text"
        @keydown.enter="send"
      ></v-text-field>
      <div class="buttons">
        <transition name="anim">
          <div class="btn" v-if="this.text === ''">
            <v-btn class="mx-2 speech" fab dark color="indigo" @click="speech">
              <v-icon>micro</v-icon>
            </v-btn>
          </div>
        </transition>
        <transition name="microAnim">
          <div class="btn" v-if="this.text !== ''">
            <v-btn class="mx-2 send" fab dark color="indigo" @click="send">
              <v-icon>send</v-icon>
            </v-btn>
          </div>
        </transition>
      </div>
    </div>
  </v-col>
</template>

<script>
export default {
  data: () => ({
    text: "",
  }),
  methods: {
    speech() {
      let transcript = "";
      const SpeechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition;
      const recognition = new SpeechRecognition();
      recognition.onresult = (event) => {
        const current = event.resultIndex;
        transcript = event.results[current][0].transcript;
        this.text += transcript;
      };
      recognition.start();
    },
    send() {
      this.$socket.emit(
        "createMessage",
        {
          text: this.text,
          id: this.$store.state.user.id,
        },
        (data) => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            this.text = "";
          }
        }
      );
    },
  },
};
</script>

<style scoped>
.form-wrapper {
  display: flex;
  justify-content: center;
}

.buttons {
  height: 58px;
}

.v-btn {
  padding: 0 0 0 5px !important;
  margin: 0;
}

.send {
  padding: 0 0 0 5px !important;
  margin: 0;
}

.speech {
  padding: 0 0 0 47px !important;
}

.anim-enter-active {
  transition-duration: 0.4s;
}

.anim-enter,
.anim-leave-to {
  opacity: 0;
  transform: translateX(50px);
  transform: scale(1.5);
}

.microAnim-enter-active {
  transition-duration: 0.4s;
}

.microAnim-enter,
.microAnim-leave-to {
  opacity: 0;
  transform: translateX(50px);
  transform: scale(1.5);
}

.form {
  width: 100%;
  flex-basis: 90%;
  left: 0;
}
</style>

