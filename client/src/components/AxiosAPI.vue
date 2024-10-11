<template>
  <div>
      <h1>Danh sach san pham</h1>
      <button @click="handleAddProduct">Them mới san phẩm</button>
    <table border="1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Ten san pham</th>
          <th>Gia</th>
          <th>So luong</th>
          <th>Mo ta</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody v-for="(product, index) in products" :key="product.id">
        <tr>
          <td>{{ index + 1 }}</td>
          <td>{{ product.name }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.quantity }}</td>
          <td>{{ product.description }}</td>
          <td>
            <button @click="handleEdit(product.id)">Edit</button>
            <button @click="handleDelete(product.id)">Delete</button>
            <button @click="getProductById(product.id)">Detail</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
const products = ref([]);
// render lay tat ca san pham
const fetchData = async () => {
  try {
    const response = await axios.get("http://localhost:8080/products");
    products.value = response.data;
  } catch (error) {
    //xu li cac loi
    console.log(error);
  }
  // axios
  // .get("http://localhost:8080/products")
  // .then((res) => {
  //   products.value = res.data;
  // })
  // .catch((err) => console.log(err));
};
onMounted(() => {
  fetchData();
});
// ham lay thong tin chi tiét 1 san pham tu id

const getProductById = async (id) => {
  try {
    const response = await axios.get(`http://localhost:8080/products/${id}`);
    console.log("Data: ", response.data);
  } catch (error) {
    console.log(error);
  }
};
const handleDelete = async (id) => {
  try {
    await axios.delete(`http://localhost:8080/products/${id}`);
    const newProducts = products.value.filter((product) => product.id !== id);
    products.value = newProducts;
  } catch (error) {
    console.log(error);
  }
};
// ham them moi san pham
 const handleAddProduct = async()=>{
    try {
        const response = await axios.post("http://localhost:8080/products", {
            name: "Men Men",
            price: 20000,
            quantity: 10,
            description: "Hoi kho",
        });
        console.log("Data: ", response.data);
        fetchData(); 
    } catch (error) {
        
    }
 }
</script>

<style></style>
