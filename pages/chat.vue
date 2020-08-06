<template>
  <div class="chat-wrapper">
    <div class="chat-msgs" ref="block">
      <Message
        v-for="m in messages"
        :key="m.text"
        :name="m.name"
        :text="m.text"
        :owner="m.id === user.id"
      />
    </div>
    <div class="chat-form">
      <ChatForm />
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import Message from "@/components/Message";
import ChatForm from "@/components/ChatForm";
export default {
  middleware: ["chat"],
  head() {
    return {
      title: `Room ${this.user.room}`,
    };
  },
  components: { Message, ChatForm },
  computed: mapState(["user", "messages"]),
  watch: {
    messages() {
      setTimeout(() => {
        this.$refs.block.scrollTop = this.$refs.block.scrollHeight;
      });
    },
  },
};
</script>

<style scoped>
.chat-wrapper {
  height: 100%;
  position: relative;
  overflow: hidden;
  background-image: url(../images/2822.jpg);
  background-size: cover;
}

.chat-form {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  height: 80px;
  transition-duration: 0.4s;
}

.chat-msgs {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 80px;
  padding: 1rem;
  overflow-y: auto;
}
</style>