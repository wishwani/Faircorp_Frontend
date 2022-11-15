<template>
  <div class="card">
    <div class="card-header">
      <h2 class="mb-0">
        <button
          class="btn btn-block text-left d-flex"
          type="button"
          @click="toggleExpand"
        >
          <div class="mr-2 text-primary">Create New Room</div>
          <div class="expand-button ml-auto text-primary">
            {{ isExpanded ? "&#9660;" : "&#9658;" }}
          </div>
        </button>
      </h2>
    </div>

    <template class="collapse show" v-if="isExpanded">
      <form class="p-4">
        <div class="form-group row">
          <label for="room_name" class="col-sm-4 col-form-label">Room Name: </label>
          <div class="col-sm-8">
            <input
              class="form-control"
              v-model="room_name"
              type="text"
              id="room_name"
              name="room_name"
              placeholder="Hall"
            />
          </div>
        </div>

        <div class="form-group row">
          <label for="room_building" class="col-sm-4 col-form-label">Room building:</label>
          <div class="col-sm-8">
            <select
              class="form-control"
              v-model="room_building"
              id="room_building"
              name="room_building"
            >
              <option v-for="building in buildings" v-bind:value="building">
                {{ building.name }}
              </option>
            </select>
          </div>
        </div>
        <div class="form-group row">
          <label for="room_current_temperature" class="col-sm-4 col-form-label">Room Current Temperature:</label>
          <div class="col-sm-8">
            <input
              class="form-control"
              type="number"
              v-model="room_current_temperature"
              id="room_current_temperature"
              name="room_current_temperature"
            />
          </div>
        </div>
        <div class="form-group row">
          <label for="room_target_temperature" class="col-sm-4 col-form-label">Room Target Temperature:</label>
          <div class="col-sm-8">
            <input
              class="form-control"
              type="number"
              v-model="room_target_temperature"
              id="room_target_temperature"
              name="room_target_temperature"
            />
          </div>
        </div>
        <div class="form-group row">
          <label for="room_floor" class="col-sm-4 col-form-label">Room Floor: </label>
          <div class="col-sm-8">
            <input
              class="form-control"
              type="number"
              v-model="room_floor"
              id="room_floor"
              name="room_floor"
            />
          </div>
        </div>
        <button type="button" class="btn btn-primary" @click="addRoom">
          Create Room
        </button>
      </form>
    </template>
  </div>
</template>

<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "RoomNew",
  data: function () {
    return {
      isExpanded: false,
      room_name: "",
      room_building: "",
      room_current_temperature: "",
      room_target_temperature: "",
      room_floor: "",
      buildings: [],
    };
  },
  created: async function () {
    let response = await axios.get(`${API_HOST}/api/buildings`);
    let buildings = response.data;
    this.buildings = buildings;
    this.room_building = buildings[0];
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async addRoom() {
      let newRoom = {
        name: this.room_name,
        buildingId: this.room_building.id,
        currentTemperature: this.room_current_temperature,
        targetTemperature: this.room_target_temperature,
        floor: this.room_floor,
      };
      let headers = {
        "Content-Type": "application/json",
      };
      let response = await axios.post(`${API_HOST}/api/rooms/`, newRoom, {
        headers,
      });
      this.$emit("room-added", response.data);
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
      top: -3px;
    }
  }

  &.closed {
    color: #dc3545;
  }
}

.room {
  .top-row {
    cursor: pointer;
  }
}
</style>
