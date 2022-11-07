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
            {{ heater.name }}
            </strong>
          </div>
          <div class="room-name text-muted pe-3 mx-2">
            Power : {{ heater.power }}
          </div>
          <div v-if="heater.roomId != null" class="room-name text-muted mx-2">
            Room Id :{{ heater.roomId }}
          </div>
          <div
            class="open-status ms-4 text-uppercase"
            :class="{ open: isHeaterOn, closed: !isHeaterOn }"
          >
            <template v-if="isHeaterOn">
              <span class="icon">&#x2B24;</span> On
            </template>
            <template v-else>
              <span class="icon">&#x2716;</span> Off
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
          <button type="button" class="btn btn-danger" @click="deleteHeater">
            Delete heater
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
  name: "HeatersListItem",
  props: ["heater", "room"],
  data: function () {
    return {
      isExpanded: false,
    };
  },
  computed: {
    isHeaterOn: function () {
      return this.heater.heaterStatus === "ON";
    },
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async deleteHeater() {
      let response = await axios.delete(
        `${API_HOST}/api/heaters/${this.heater.id}`
      );
      this.$emit("heater-deleted", this.heater.id);
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

.heater {
  .top-row {
    cursor: pointer;
  }
}
</style>
