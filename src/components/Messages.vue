<template>
  <div>
    <h3>Display Messages</h3>
    <input type="text" placeholder="enter name" v-model="name" />
    <br />
    <br />
    <div>
      <input type="date" v-model="starttime" />
    </div>
    <br />
    <div>
      <input type="date" v-model="endtime" />
    </div>
    <br />
    <button v-on:click="retrieveSender()">Sender Messages</button>
    <button v-on:click="retrieveReceiver()">Receiver Messages</button>
    <Messagelist v-bind:messagelist="messages" />
    <button v-if="counter > 0" v-on:click="prevbutton()">prev</button>
    <button v-if="nextbuttonenable" v-on:click="nextbutton()">next</button>
  </div>
</template>

<script>
import Messagelist from "./Messagelist.vue";
import axios from "axios";
export default {
  name: "Messages",
  data() {
    return {
      name: null,
      starttime: new Date(),
      endtime: new Date(),
      counter: 0,
      retreive: null,
      nextbuttonenable: false,
      cached: false,
      messages: null,
    };
  },
  components: {
    Messagelist,
  },
  methods: {
    prevbutton: async function() {
      this.counter = this.counter - 1;
      this.cached = true;
      await this.retreive();
    },
    nextbutton: async function() {
      this.counter = this.counter + 1;
      this.cached = true;
      // console.log(this.retreive);
      await this.retreive();
      // debugger;
      // await console.log("hjks");
    },
    retrieveSenderUtil: function() {
      // console.log("here");
      axios
        .post(`/retrieveSender`, {
          counter: this.counter,
          name: this.name,
          start: this.starttime,
          end: this.endtime,
          cached: this.cached,
        })
        .then((res) => {
          console.log(res.data);
          this.messages = res.data;
        });
    },
    retrieveSender: function() {
      this.counter = 0;
      this.retreive = this.retrieveSenderUtil;
      // console.log(this.retreive);
      this.retreive();
      this.nextbuttonenable = true;
      // console.log(this.retreive);
    },
    retrieveReceiverUtil: function() {
      axios
        .post(`/retrieveReceiver`, {
          name: this.name,
          start: this.starttime,
          end: this.endtime,
          counter: this.counter,
          cached: this.cached,
        })
        .then((res) => {
          console.log(res.data);
          this.messages = res.data;
        });
    },
    retrieveReceiver: function() {
      this.counter = 1;
      this.retreive = this.retrieveReceiverUtil;
      this.retreive();
      this.nextbuttonenable = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
