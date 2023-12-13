<script>
import axios from "axios";
export default {
  data: function () {
    return {
      rooms: [],
      newRoomParams: {},
      editRoomParams: {},
      currentRoom: {},
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
    showRoom: function (room) {
      this.currentroom = room;
      this.editRoomParams = room;
      document.querySelector("#room-details").showModal();
    },
    updateRoom: function (room) {
      axios
        .patch("/rooms/" + room.id + ".json", this.editRoomParams)
        .then((response) => {
          console.log("rooms update", response);
          this.currentRoom = {};
        })
        .catch((error) => {
          console.log("rooms update error", error.response);
        });
    },
    destroyRoom: function (room) {
      axios.delete("/rooms/" + room.id + ".json").then((response) => {
        console.log("rooms destroy", response);
        var index = this.rooms.indexOf(room);
        this.rooms.splice(index, 1);
      });
    },
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
      <button v-on:click="showRoom(room)">More info</button>
    </div>
    <dialog id="room-details">
      <form method="dialog">
        <h1>Room info</h1>
        <p>
          Name:
          <input type="text" v-model="editRoomParams.name" />
        </p>
        <p>
          City:
          <input type="text" v-model="editRoomParams.city" />
        </p>
        <p>
          State:
          <input type="text" v-model="editRoomParams.state" />
        </p>
        <button v-on:click="updateRoom(currentRoom)">Update</button>
        <button v-on:click="destroyRoom(currentRoom)">Destroy Room</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
