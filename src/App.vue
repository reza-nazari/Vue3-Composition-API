<template>
    <div id="app">
        <button id="toggleUserBtn" @click="toggleUserInfo">
            {{ showUserInfo ? "hide" : "show" }} user info</button
        >
        <p v-if="showUserInfo">Welcome! your logged in</p>
        <hr>
        <p>Add New Product!</p>
        <p>You can remove product by click</p>

        <ProductForm :createProduct="createProduct" />
        <Products :items="products" :remove="deleteProduct" />
    </div>
</template>

<script>
import { reactive, ref } from "@vue/composition-api";
import { useToggle } from "./compositions/toggle";

import ProductForm from "./components/ProductForm";
import Products from "./components/Products";

export default {
    name: "App",
    components: {
        ProductForm,
        Products,
    },
    setup() {
        let products = ref([]);

        const createProduct = (title, price) => {
            const newProduct = {
                title: title,
                price: price,
                id: Math.random(),
            };

            products.value.push(newProduct);
        };

        const deleteProduct = (productId) => {
            products.value = products.value.filter((p) => p.id !== productId);
        };

        const { show: showUserInfo, toggle: toggleUserInfo } = useToggle();

        return {
            products,
            showUserInfo,
            createProduct,
            deleteProduct,
            toggleUserInfo,
        };
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
}

* {
    box-sizing: border-box;
}

#toggleUserBtn,
p {
    margin: 0 5%;
}

img {
    margin: 5px auto;
    display: block;
}
</style>
