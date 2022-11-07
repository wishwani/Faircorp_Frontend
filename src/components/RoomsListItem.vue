<template>
  <div class="card">
    <div class="card-header">
      <h2 class="mb-0">
        <button
          class="btn btn-link btn-block text-left d-flex flex-column flex-sm-row"
          type="button"
          @click="toggleExpand"
        >
          <div class="mr-2">
            <strong>
              {{ room.name }}
            </strong>
          </div>
          <div
            v-if="room.currentTemperature != null"
            class="room-name text-muted pe-3 mx-2"
          >
            Current Temperature : {{ room.currentTemperature }}
          </div>
          <div
            v-if="room.targetTemperature != null"
            class="room-name text-muted pe-3 mx-2"
          >
            Target Temperature : {{ room.targetTemperature }}
          </div>

          <div class="expand-button ml-auto">
            {{ isExpanded ? "&#9660;" : "&#9658;" }}
          </div>
        </button>
      </h2>
    </div>

    <template class="collapse show" v-if="isExpanded">
      <div class="card-body">
        <div class="room-info mb-3">
          <div class="room-name text-muted pe-3">ID : {{ room.id }}</div>
          <div class="room-name text-muted pe-3">
            Building : {{ room.buildingName }}
          </div>
          <div class="room-name text-muted pe-3">Floor : {{ room.floor }}</div>
          <div
            v-if="room.currentTemperature != null"
            class="room-name text-muted pe-3"
          >
            Current Temperature : {{ room.currentTemperature }}
          </div>
          <div
            v-if="room.targetTemperature != null"
            class="room-name text-muted pe-3"
          >
            Target Temperature : {{ room.targetTemperature }}
          </div>
        </div>
        <div class="details d-flex">
          <button
            type="button"
            class="btn btn-secondary me-2 mr-3"
            @click="switchWindows"
          >
            Switch Windows
          </button>
          <button
            type="button"
            class="btn btn-secondary me-2 mr-3"
            @click="switchHeaters"
          >
            Switch Heaters
          </button>
          <button type="button" class="btn btn-danger me-2" @click="deleteRoom">
            Delete room
          </button>
        </div>
        <div class="windows-list mt-4">
          <windows-list-item
            v-for="window in room.windowList"
            :window="window"
            :key="window.id"
            :room="room"
            @window-updated="updateWindow"
            @window-deleted="deleteWindow"
          >
          </windows-list-item>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import axios from "axios";
import { API_HOST } from "../config";
import WindowsListItem from "./WindowsListItem";

export default {
  components: {
    WindowsListItem,
  },
  name: "RoomsListItem",
  props: ["room"],
  data: function () {
    return {
      isExpanded: false,
    };
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async deleteRoom() {
      let response = await axios.delete(
        `${API_HOST}/api/rooms/${this.room.id}`
         
      );
      this.$emit("room-deleted", this.room.id);
    },
    async switchWindows() {
      let response = await axios.put(
        `${API_HOST}/api/rooms/${this.room.id}/switchWindow`
      );
      this.$emit("room-updated", response.data);
    },
    async switchHeaters() {
      let response = await axios.put(
        `${API_HOST}/api/rooms/${this.room.id}/switchHeater`
      );
      this.$emit("room-updated", response.data);
    },
    updateWindow(newWindow) {
      /* Find the place of window object with the same Id in the array, and replace it */
      let index = this.room.windowList.findIndex(
        (window) => window.id === newWindow.id
      );
      this.room.windowList.splice(index, 1, newWindow);
    },
    deleteWindow(windowId) {
      let index = this.room.windowList.findIndex(
        (window) => window.id === windowId
      );
      if (index > -1) {
        this.room.windowList.splice(index, 1);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.room {
  .top-row {
    cursor: pointer;
  }
}
</style>
