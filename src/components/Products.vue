<template>
    <ul>
        <li
            v-for="prod of items"
            :key="prod.id"
            @click="removeProduct(prod.id)"
        >
            <h2> {{ prod.title }} </h2>
            <h3> Price: ${{ prod.price }} </h3>
        </li>
    </ul>
</template>

<script>
export default {
    props: {
        items: {
            type: Array,
            required: true,
        },
        remove: {
            type: Function,
            required: true,
        },
    },

    methods: {
        removeProduct(productId) {
            this.remove(productId);
        },
    },
};
</script>

<style>
ul {
    width: 90%;
    margin: 10px auto;
    padding: 0;
}
li {
    list-style: none;
    border: 1px solid orangered;
    border-radius: 10px;
    background-color: lightcoral;
    padding: 5px;
    margin-bottom: 10px;
    box-shadow: 2px 2px 10px #ccc;
}
</style>
