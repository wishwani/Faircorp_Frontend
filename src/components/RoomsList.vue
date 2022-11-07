<template>
<div class="accordion" id="accordion">
    <rooms-list-item 
      v-for="room in rooms"
      :room="room"
      :key="room.id"
      @room-updated="updateRoom"
      @room-deleted="deleteRoom"
    >
    </rooms-list-item>
    <room-new
      @room-added="addRoom"
    >
    </room-new>
</div>
  
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomsListItem from './RoomsListItem';
import RoomNew from './RoomNew.vue';

export default {
  components: {
      RoomsListItem,
      RoomNew
  },
  name: 'RoomsList',
  data: function() {
    return {
      /* Initialize rooms with an empty array, while waiting for actual data to be retrieved from the API */
      rooms: []
    }
  },
  created: async function() {
    /*let response = await axios.get(`${API_HOST}/api/rooms`);*/
    let response = await axios.get(`${API_HOST}/api/rooms`);
    let rooms = response.data;
    this.rooms = rooms;
  },
  methods: {
    deleteRoom(roomId) {
      let index = this.rooms.findIndex(room => room.id === roomId);
      if (index > -1) {
	this.rooms.splice(index, 1);
      }
    },
      addRoom(newRoom) {
	  this.rooms.push(newRoom);
      },
      updateRoom(newRoom) {
	  let index = this.rooms.findIndex(room => room.id === newRoom.id);
	  this.rooms.splice(index, 1, newRoom);
	  }
  }
}
</script>
