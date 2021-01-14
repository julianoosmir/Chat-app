<template>
  <div id="app" class="container" >
    <form class="row"  @submit.prevent="send" id="Chat">
      <input type="text" v-model="username" placeholder="digite o usuario" />
      <div id="tela" class="messages">
        <div v-for="men in messages" :key="men.user">
          <strong>{{ men.user }}</strong> : {{ men.message }}
        </div>
      </div>
      <input
        type="text"
        v-model="newMessage"
        placeholder="digite a menssagem"
      />
      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
import io from "socket.io-client";
export default {
  name: "App",
  components: {},
  data() {
    return {
      messages: [],
      newMessage: null,
      username: null,
      socket: io("http://localhost:3000"),
    };
  },
  methods: {
    async send() {
      this.messages.push({
        message: this.newMessage,
        user: this.username,
      });

      this.socket.emit("sendMenssage", {
        message: this.newMessage,
        user: this.username,
      });
      this.receber();

      this.socket.on("previousMessages", (data) => {
        this.messages = data;
      });
    },
    receber() {
      this.socket.on("receivedMessage", (data) => {
        this.messages = data;
      });

      this.socket.on("previousMessages", (data) => {
        this.messages = data;
      });
    },
  },
  created() {
    this.receber();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 50px; margin-right: 800px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#chat {
  height: 80%;
  display: inline;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

input {
  width: 600px;
  border: 1px solid #ddd;
  height: 50px;
  padding: 0 20px;
  font-size: 14px;
}

button {
  width: 600px;
  height: 50px;
  font-size: 14px;
  background: #069;
  text-align: center;
  line-height: 50px;
  font-weight: bold;
  color: #fff;
  margin-top: 10px;
}

.messages {
  width: 600px;
  height: 400px;
  margin: 20px 0;
  border: 1px solid #ddd;
  padding: 20px;
}
</style>
