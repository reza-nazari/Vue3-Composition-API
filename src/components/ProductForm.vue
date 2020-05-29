<template>
    <form id="productForm" @submit="saveProduct">
        <div class="form-control">
            <label for="title">Title</label>
            <input type="text" id="title" v-model="inputState.title" />
        </div>

        <div class="form-control">
            <label for="price">Price</label>
            <input
                type="number"
                min="0"
                step="0.01"
                id="price"
                v-model="inputState.price"
            />
        </div>
        <button type="submit" :disabled="!isValid">Save</button>
    </form>
</template>

<script>
import { ref, reactive, watchEffect, computed } from "@vue/composition-api";

export default {
    props: {
        createProduct: {
            type: Function,
            required: true,
        },
    },
    setup(props) {
        const inputState = reactive({
            title: "",
            price: "",
        });
        const submitted = ref(false);

        const priceAsNumber = computed(() => {
            return parseFloat(inputState.price);
        });

        const isValid = computed(() => {
            let isValid = true;

            if (inputState.title.trim().length == 0) {
                isValid = false;
            }

            if (isNaN(priceAsNumber.value) || priceAsNumber.value <= 0) {
                isValid = false;
            }

            return isValid;
        });

        watchEffect(() => {
            if (submitted.value) {
                inputState.title = "";
                inputState.price = "";
                submitted.value = false;
            }
        });

        const saveProduct = (event) => {
            event.preventDefault();
            props.createProduct(inputState.title, inputState.price);
            submitted.value = true;
        };

        return {
            inputState,
            isValid,
            saveProduct,
        };
    },
};
</script>

<style>
#productForm {
    width: 90%;
    margin: 10px auto;
    border: 1px solid lightgrey;
    border-radius: 10px;
    padding: 5px;
    box-shadow: 2px 2px 10px #ccc;
}

.form-control {
    display: block;
    position: relative;
    margin: 5px;
}

input {
    width: 100%;
    padding: 10px;
    border: 1px solid lightgrey;
}

button {
    margin: 5px;
    padding: 10px;
    border-radius: 10px;
    border: none;
    box-shadow: 1px 1px 5px #ccc;
    background-color: lightseagreen;
    color: #fff;
}

button:disabled{
    color: grey;
    background-color: lightsteelblue;
}
</style>
