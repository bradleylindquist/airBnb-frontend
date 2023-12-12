<script>
import axios from "axios";
export default {
  data: function () {
    return {
      rooms: [],
      newRoomParams: {},
    };
  },
  created: function () {
    this.indexRooms();
  },
  methods: {
    indexRooms: function () {
      axios.get("/rooms.json").then((response) => {
        console.log("rooms index", response);
        this.rooms = response.data;
      });
    },
  },
  createRoom: function () {
    axios
      .post("/rooms.json", this.newRoomParams)
      .then((response) => {
        console.log("rooms create", response);
        this.rooms.push(response.data);
        this.newRoomParams = {};
      })
      .catch((error) => {
        console.log("rooms create error", error.response);
      });
  },
};
</script>

<template>
  <div class="home">
    <h1>New Room</h1>
    <div>
      Name:
      <input type="text" v-model="newRoomParams.name" />
      City:
      <input type="text" v-model="newRoomParams.city" />
      State:
      <input type="text" v-model="newRoomParams.state" />
      <button v-on:click="createRoom()">Create Room</button>
    </div>
    <h1>All Rooms</h1>
    <div v-for="room in rooms" v-bind:key="room.id">
      <h2>{{ room.name }}</h2>
      <img v-bind:src="room.url" v-bind:alt="room.name" />
      <p>City: {{ room.city }}</p>
      <p>State: {{ room.state }}</p>
    </div>
  </div>
</template>

<style></style>
