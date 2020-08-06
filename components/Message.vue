<template>
  <div>
    <div v-if="name === 'admin'" class="system">
      <p class="system-text-center">{{text}}.</p>
      <hr />
    </div>
    <div v-else class="msg-wrapper">
      <div class="msg" :class="{owner}">
        <div class="name">{{name}}</div>
        <p class="text">{{text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    text: String,
    owner: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style>
.system {
  margin: 0 0 10px 0;
}

.primary--text {
  color: white !important;
  caret-color: white !important;
}

.system-text-center {
  margin: 0 0 5px 0 !important;
  font-size: 17px !important;
  font-family: "Dance", cursive;
}

.msg-wrapper {
  display: flex;
  flex-direction: column;
}

.text {
  font-family: "Dance", cursive;
  font-size: 15px;
}

.msg {
  padding: 1rem;
  max-width: 55%;
  margin: 0 1rem;
  box-shadow: 0 1px 0 0 rgba(50, 50, 50, 0.3);
  border-radius: 4px;
  background: #1976d2;
  margin: 0 0 1rem 0;
  position: relative;
  display: flex;
  flex-direction: column;
  overflow-x: hidden;
}

.name {
  font-size: 16px;
  font-family: "Ranchers", cursive;
  font-weight: 700;
}

p {
  margin-bottom: 0;
}

.owner {
  background-color: rgb(255, 255, 255);
  color: #000;
  align-self: flex-end;
}

.system-text-center {
  font-size: 16px;
  display: flex;
  justify-content: center;
  padding: 1rem;
}

@font-face {
  font-family: "Ranchers";
  src: url("~@/fonts/Ranchers-Regular.ttf") format("ttf");
}

@font-face {
  font-family: "Dance";
  src: url("~@/fonts/DancingScript-VariableFont_wght.ttf") format("ttf");
}

@font-face {
  font-family: "Rowdies";
  src: url("~@/fonts/Rowdies-Bold.ttf") format("ttf");
}

@font-face {
  font-family: "Ranchers";
  src: url("~@/fonts/Ranchers-Regular.ttf") format("ttf");
}
</style>