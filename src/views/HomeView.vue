<template>
  <div class="home">
    <a class="logout-button gap" @click="logout">Logout</a>
    <div class="gap" v-if="user">
      <img :src="user.avatar" alt="User Avatar" />
      <h1>{{ user.name }}</h1>
      <p>Email: {{ user.email }}</p>
      <p>Role: {{ user.role }}</p>
    </div>
    <table>
      <thead>
        <tr>
          <th>No</th>
          <th>Title</th>
          <th>Image</th>
          <th>Description</th>
          <th>Category</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in dataProduct" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>
            <img :src="item.images" style="width: 50px" alt="" />
          </td>
          <td>{{ item.description }}</td>
          <td>{{ item.category }}</td>
          <td>{{ item.price }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// @ is an alias to /src
import cookie from "js-cookie";
import { mapGetters } from "vuex";

export default {
  name: "HomeView",
  data() {
    return {
      dataProduct: [],
    };
  },
  mounted() {
    this.getListDataProduct();
    console.log(this.dataProduct);
  },
  methods: {
    logout() {
      cookie.remove("userdata"); // Remove the cookie named userdata
      this.$store.commit("SET_LOGOUT"); // Trigger SET_LOGOUT in store/index
      this.$router.push({ path: "/" });
    },
    getListDataProduct() {
      this.$axios
        .get("products", {
          params: {
            limit: 20,
            offset: 5,
          },
        })
        .then((res) => {
          let productData = res.data;
          // let dataImage = res.data.image[0];
          console.log("ini data asli: ", productData); //ini data asli
          // this.dataProduct = res.data;
          productData.forEach((item) => {
            this.dataProduct.push({
              category: item.category.name,
              name: item.title,
              price: item.price,
              description: item.description,
              images: item.category.image,
            });
          });
          console.log("ini data hasil manipulasi: ", this.dataProduct);
        });
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
