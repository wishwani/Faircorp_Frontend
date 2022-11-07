<template>
  <div class="accordion" id="accordion">
    <heaters-list-item 
      v-for="heater in heaters"
      :heater="heater"
      :key="heater.id"  
      @heater-updated="updateHeater"
      @heater-deleted="deleteHeater"
    >
    </heaters-list-item>
    <heater-new
      @heater-added="addHeater"
    >
    </heater-new>
</div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import HeatersListItem from './HeatersListItem';
import HeaterNew from './HeaterNew.vue';

export default {
  components: {
      HeatersListItem,
      HeaterNew
  },
  name: 'HeatersList',
  data: function() {
    return {
      /* Initialize heaters with an empty array, while waiting for actual data to be retrieved from the API */
      heaters: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/heaters`);
    let heaters = response.data;
    this.heaters = heaters;
  },
  methods: {
    updateHeater(newHeater) {
      /* Find the place of heater objectw ith the same Id in the array, and replace it */
      let index = this.heaters.findIndex(heater => heater.id === newHeater.id);
      this.heaters.splice(index, 1, newHeater);
    },
    deleteHeater(heaterId) {
      let index = this.heaters.findIndex(heater => heater.id === heaterId);
      if (index > -1) {
	this.heaters.splice(index, 1);
      }
    },
      addHeater(newHeater) {
	this.heaters.push(newHeater);
    }
  }
}
</script>
