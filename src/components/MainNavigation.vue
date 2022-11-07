<template>
<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
  <a class="navbar-brand" href="#"><img class="logo"  height="30" src="..\assets\school_logo.svg"></a>
  <a class="navbar-brand" href="#">{{ title }}</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarColor01" aria-controls="navbarColor01" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarColor01">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item" :class="{active: isCurrentNav('windows')}">
        <a class="nav-link test" href="javascript:void(0);" @click="changeNav('windows')">Windows</a>
      </li>
      <li class="nav-item" :class="{active: isCurrentNav('rooms')}">
        <a class="nav-link" href="javascript:void(0);" @click="changeNav('rooms')">Rooms</a>
      </li>
      <li class="nav-item" :class="{active: isCurrentNav('rooms')}">
        <a class="nav-link" href="javascript:void(0);" @click="changeNav('heaters')">Heaters</a>
      </li>
    </ul>

  </div>
</nav>

</template>


<script>
export default {
  name: 'MainNavigation',
  props: ['title'],
  data: function() {
    return {
      currentNavItem: 'windows'
    }
  },
  methods: {
    changeNav(item) {
      this.currentNavItem = item;
      this.$emit('nav-updated',item);
    },
    isCurrentNav(item) {
      return this.currentNavItem === item;
    }
  }
}
</script>
<style scoped>
.navbar-brand{
  font-family: 'impact', 'Arial Narrow', Arial, sans-serif;
}
.logo {
  width:60px
}
</style>
