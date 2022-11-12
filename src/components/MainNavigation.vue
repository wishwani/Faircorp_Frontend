<template>
<nav class="navbar navbar-expand-lg navbar-dark bg-secondary">
  <a class="navbar-brand" href="#"><img class="logo" src="..\assets\mines-logo.svg">{{ title }}</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollection" aria-controls="√" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarCollection">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item" :class="{active: isCurrentNav('windows')}">
        <a class="nav-link test" href="javascript:void(0);" @click="changeNav('windows')">Windows</a>
      </li>
      <li class="nav-item" :class="{active: isCurrentNav('rooms')}">
        <a class="nav-link" href="javascript:void(0);" @click="changeNav('rooms')">Rooms</a>
      </li>
      <li class="nav-item" :class="{active: isCurrentNav('heaters')}">
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
.navbar {
  margin-bottom: 30px;
  display: flex;
  flex-direction: column;
}
.navbar-brand{
  font-family: "Gill Sans", sans-serif;
}
.logo {
  height: 80px;
  width: 80px;
}
</style>
