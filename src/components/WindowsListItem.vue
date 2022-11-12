<template>
  <div class="card">
    <div class="card-header">
      <h2 class="mb-0">
        <button
          class="btn btn-block text-left d-flex flex-column flex-sm-row"
          type="button"
          @click="toggleExpand"
        >
          <div class="mr-2">
            <strong>
            {{ window.name }}
            </strong>
          </div>
          <div v-if="window.roomId != null" class="room-name text-muted mx-2">
            {{ window.roomName }}
          </div>
          <div v-else class="room-name text-muted mx-3">{{ roomName }}</div>
          <div
            class="open-status ms-4 text-uppercase"
            :class="{ open: isWindowOpen, closed: !isWindowOpen }"
          >
            <template v-if="isWindowOpen">
              <span class="icon">&#x2B24;</span> Open
            </template>
            <template v-else>
              <span class="icon">&#x2716;</span> Closed
            </template>
          </div>

          <div class="expand-button ml-auto">
            {{ isExpanded ? "&#9660;" : "&#9658;" }}
          </div>
        </button>
      </h2>
    </div>

    <template
      class="collapse show"
      v-if="isExpanded"
    >
      <div class="card-body" >
        <div class="details d-flex">
          <button
            type="button"
            class="btn btn-secondary me-2 mr-3"
            @click="switchWindow"
          >
            {{ isWindowOpen ? "Close" : "Open" }} window
          </button>
          <button type="button" class="btn btn-danger" @click="deleteWindow">
            Delete window
          </button>
        </div>
      </div>
    </template>
  </div>

</template>

<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "WindowsListItem",
  props: ["window", "room"],
  data: function () {
    return {
      isExpanded: false,
    };
  },
  computed: {
    isWindowOpen: function () {
      return this.window.windowStatus === "OPEN";
    },
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchWindow() {
      let response = await axios.put(`${API_HOST}/api/windows/${this.window.id}/switch`);
      let updatedWindow = response.data;
      this.$emit('window-updated', updatedWindow);
    },
    async deleteWindow() {
      let response = await axios.delete(
         `${API_HOST}/api/windows/${this.window.id}`
      );
      this.$emit("window-deleted", this.window.id);
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

.window {
  .top-row {
    cursor: pointer;
  }
}
</style>
