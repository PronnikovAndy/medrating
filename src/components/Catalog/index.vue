<template>
  <Users
      :items="users"
  />
</template>

<script>
import Users from "./Users";

export default {
  name: 'Catalog',
  data() {
    return {
      users: [],
      loading: true
    }
  },
  mounted() {
    fetch('https://json.medrating.org/users/')
        .then(response => response.json())
        .then(data => data.filter(t => t.name))
        .then(data => this.users = data);
  },
  components: {
    Users
  }
}
</script>

<style>
ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

li {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 10px;
  margin: 10px 0;
}

li span:hover {
  cursor: pointer;
}

.arrow-right:before {
  content: '\003E';
  margin-right: 10px;
}

.arrow-down:before {
  content: '\02C5';
  margin-right: 10px;
}
</style>