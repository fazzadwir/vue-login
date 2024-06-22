<template>
  <div class="home">
    <a class="logout-button gap" @click="logout">Logout</a>
    <div class="gap" v-if="user">
      <img :src="user.avatar" alt="User Avatar" />
      <h1>{{ user.name }}</h1>
      <p>Email: {{ user.email }}</p>
      <p>Role: {{ user.role }}</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import cookie from "js-cookie";
import { mapGetters } from "vuex";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
  methods: {
    logout() {
      cookie.remove("userdata"); //hapus cookie yang namanya userdata
      this.$store.commit("SET_LOGOUT"); // trigger SET_LOGOUT di store/index
      this.$router.push({ path: "/" });
    },
  },
  computed: {
    ...mapGetters(["getUser"]),
    user() {
      const userdata = cookie.get("userdata");
      return userdata ? JSON.parse(userdata) : null;
    },
  },
};
</script>

<style>
.logout-button {
  background-color: #b23b3b;
  color: white;
  font-size: 18px;
  font-weight: 700;
  padding: 16px 24px;
  border-radius: 24px;
  margin-bottom: 24px;
}
.gap {
  margin: 24px;
}
</style>
