<template>
  <div class="accordion" id="accordion">
    <windows-list-item 
      v-for="window in windows"
      :window="window"
      :key="window.id"  
      @window-updated="updateWindow"
      @window-deleted="deleteWindow"
    >
    </windows-list-item>
    <window-new
      @window-added="addWindow"
    >
    </window-new>
</div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import WindowsListItem from './WindowsListItem';
import WindowNew from './WindowNew.vue';

export default {
  components: {
      WindowsListItem,
      WindowNew
  },
  name: 'WindowsList',
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      windows: []
    }
  },
  created: async function() {
    /*let response = await axios.get(`${API_HOST}/window`);*/
   let response = await axios.get(`${API_HOST}/api/windows`);
    let windows = response.data;
    this.windows = windows;
  },
  methods: {
    updateWindow(newWindow) {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      let index = this.windows.findIndex(window => window.id === newWindow.id);
      this.windows.splice(index, 1, newWindow);
    },
    deleteWindow(windowId) {
      let index = this.windows.findIndex(window => window.id === windowId);
      if (index > -1) {
	this.windows.splice(index, 1);
      }
    },
      addWindow(newWindow) {
	this.windows.push(newWindow);
    }
  }
}
</script>
