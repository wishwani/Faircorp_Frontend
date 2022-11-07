<template>
  <div class="card">
    <div class="card-header">
      <h2 class="mb-0">
        <button
          class="btn btn-link btn-block text-left d-flex"
          type="button"
          @click="toggleExpand"
        >
          <div class="mr-2">Create New Window</div>
          <div class="expand-button ml-auto ">
            {{ isExpanded ? "&#9660;" : "&#9658;" }}
          </div>
        </button>
      </h2>
    </div>

    <template class="collapse show" v-if="isExpanded">
      <form class="p-4">
        <div class="form-group">
          <label for="window_name">Window Name: </label>
          <input
            v-model="window_name"
            type="text"
            class="form-control"
            name="window_name"
            id="window_name"
            placeholder="window"
          />
        </div>
        <div class="form-group">
          <label for="window_status">Window Status: </label>
          <select
            v-model="window_status"
            class="form-control"
            id="window_status"
            name="window_status"
          >
            <option value="CLOSED">Closed</option>
            <option value="OPEN">Open</option>
          </select>
        </div>
        <div class="form-group">
          <label for="window_room">Window Room: </label>
          <select
            v-model="window_room"
            class="form-control"
            id="window_room"
            name="window_room"
          >
            <option v-for="room in rooms" v-bind:value="room">
              {{ room.name }}
            </option>
          </select>
        </div>
        <button type="button" class="btn btn-primary" @click="addWindow">
          Create Window
        </button>
      </form>
    </template>
  </div>
</template>


<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "WindowNew",
  data: function () {
    return {
      isExpanded: false,
      window_name: "",
      window_status: "CLOSED", 
      roomId: "",
      window_room: "",
      rooms: [],
    };
  },
  created: async function () {
    let response = await axios.get(`${API_HOST}/api/rooms`);
    let rooms = response.data;
    this.rooms = rooms;
    this.window_room = rooms[0];
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async addWindow() {
      let newWindow = {
        name: this.window_name,
        roomId: this.window_room.id,
        room: this.window_room,
        windowStatus: this.window_status,
      };
      let headers = {
        "Content-Type": "application/json",
      };
      let response = await axios.post(`${API_HOST}/api/windows`, newWindow, {
        headers,
      });
      this.$emit("window-added", response.data);
    },
  },
};
</script>

<style lang="scss" scoped>
.open-status {
  .icon {
    position: relative;
  }

  &.open {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -2px;
    }
  }

  &.closed {
    color: #dc3545;
  }
}

.window {
  .top-row {
    cursor: pointer;
  }
}
</style>
