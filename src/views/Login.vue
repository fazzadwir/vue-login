<template>
  <form @submit.prevent="onSubmit">
    <div class="container">
      <h1>Login</h1>
      <input
        class="input-field"
        placeholder="Insert your email"
        type="text"
        v-model="email"
      />
      <input
        class="input-field"
        value="changeme"
        placeholder="Insert your password"
        type="password"
        v-model="password"
      />
      <input class="submit-button" type="submit" />
    </div>
  </form>
</template>
<script>
import cookie from "js-cookie";
export default {
  data() {
    return {
      email: "john@mail.com",
      password: "changeme",
    };
  },
  methods: {
    onSubmit() {
      this.$axios
        .post("/auth/login", {
          email: this.email,
          password: this.password,
        })
        .then((response) => {
          this.getDataUser(response.data);
        });
    },
    getDataUser(data) {
      this.$axios
        .get("auth/profile", {
          headers: {
            Authorization: "Bearer " + data.access_token,
          },
        })
        .then((res) => {
          let userdata = Object.assign(res.data, data);
          let forcookie = JSON.stringify(userdata);
          cookie.set("userdata", forcookie, { expires: 1 });
          console.log(userdata);
          this.$store.commit("SET_LOGIN", forcookie);
          this.$router.push({ path: "/home" });
        });
    },
  },
};
</script>

<style>
.container {
  background-color: #fbf8f6;
  height: 20vw;
  width: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  padding: 16px 24px;
  border: 0.25px solid grey;
  border-radius: 30px;
}

.input-field {
  background-color: white;
  border-radius: 12px;
  margin-right: 24px;
  padding: 16px 24px;
  font-size: 18px;
  width: 80%;
}
.submit-button {
  background-color: #715d4e;
  border-radius: 12px;
  padding: 16px 24px;
  color: white;
  font-size: 18px;
  font-weight: 600;
  border-radius: 40px;
  border: 0;
  width: 100%;
}
body {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #d6beaa;
  height: 50vw;
}
</style>
