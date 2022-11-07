<template>
  <div class="card">
    <div class="card-header">
      <h2 class="mb-0">
        <button
          class="btn btn-link btn-block text-left d-flex"
          type="button"
          @click="toggleExpand"
        >
          <div class="mr-2">Create New Heater</div>
          <div class="expand-button ml-auto ">
            {{ isExpanded ? "&#9660;" : "&#9658;" }}
          </div>
        </button>
      </h2>
    </div>

    <template class="collapse show" v-if="isExpanded">
      <form class="p-4">
        <div class="form-group">
          <label for="heater_name">Heater Name: </label>
          <input
            v-model="heater_name"
            type="text"
            class="form-control"
            name="heater_name"
            id="heater_name"
            placeholder="heater"
          />
        </div>
        <div class="form-group">
          <label for="heater_power">Heater Power: </label>
          <input
            v-model="heater_power"
            type="text"
            class="form-control"
            name="heater_power"
            id="heater_power"
            placeholder="power"
          />
        </div>
        <div class="form-group">
          <label for="heater_status">Heater Status: </label>
          <select
            v-model="heater_status"
            class="form-control"
            id="heater_status"
            name="heater_status"
          >
            <option value="OFF">OFF</option>
            <option value="ON">ON</option>
          </select>
        </div>
        <div class="form-group">
          <label for="heater_room">Heater Room: </label>
          <select
          v-model="heater_room"
            class="form-control"
            
            id="heater_room"
            name="heater_room"
          >
            <option v-for="room in rooms" v-bind:value="room">
              {{ room.name }}
            </option>
          </select>
        </div>
        <button type="button" class="btn btn-primary" @click="addHeater">
          Create Heater
        </button>
      </form>
    </template>
  </div>
</template>


<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "HeaterNew",
  data: function () {
    return {
      isExpanded: false,
      heater_name: "",
      heater_power: "",
      heater_status: "OFF",
      heater_room: "",
      rooms: [],
    };
  },
  created: async function () {
    let response = await axios.get(`${API_HOST}/api/rooms`);
    let rooms = response.data;
    this.rooms = rooms;
    this.heater_room = rooms[0];
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async addHeater() {
      let newHeater = {
        name: this.heater_name,
        power: this.heater_power,
        roomId: this.heater_room.id,
        room: this.heater_room,
        heaterStatus: this.heater_status,
      };
      let headers = {
        "Content-Type": "application/json",
      };
      let response = await axios.post(`${API_HOST}/api/heaters`, newHeater, {
        headers,
      });
      this.$emit("heater-added", response.data);
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

.heater {
  .top-row {
    cursor: pointer;
  }
}
</style>
