<template>
  <div id="app">
    <form>
      <h3>Enter Messages</h3>
      <input type="text" placeholder="from" v-model="sender" />
      <br />
      <br />
      <input type="text" placeholder="to" v-model="receiver" />
      <br />
      <br />
      <input type="text" placeholder="message" v-model="message" />
      <br />
      <br />
      <button v-on:click.prevent="submit()">Submit</button>
      <br />
      <br />
    </form>
    <Messages />
  </div>
</template>

<script>
import Messages from "./components/Messages.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    Messages,
  },
  data() {
    return {
      sender: null,
      receiver: null,
      message: null,
    };
  },
  methods: {
    submit: function() {
      axios
        .post(`/${this.sender}`, {
          sender: this.sender,
          receiver: this.receiver,
          message: this.message,
        })
        .then((res) => {
          console.log(res.data);
        });
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}
</style>
