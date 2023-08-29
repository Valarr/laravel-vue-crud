<template>
    <div class="col-12 col-md-6 p-4 mx-auto">
        <ul class="list-unstyled border-top mt-3 cursor-pointer">
            <li class="py-3 border-bottom text-gray-600 group product" v-for="product in products" :key="product.id">
                <div class="rounded-md p-2 max-w-xl w-100 hover-bg-gray-400">
                    <div class="d-flex gap-4">
                        <div>
                            <img :src="product.thumbnail" :alt="product.code" class="rounded embed-responsive embed-responsive-1by1" height="170"/>
                        </div>
                        <div class="flex-grow-1">
                            <div class="d-flex justify-content-between gap-4 w-100">
                                <span>
                                    [{{ product.code }}] {{ product.name }}
                                </span>
                                <span>
                                    {{
                                        Intl.NumberFormat("pt-BR", {
                                            style: "currency",
                                            currency: "BRL",
                                        }).format(product.price / 100)
                                    }}
                                </span>
                            </div>
                            <div class="h-2 rounded">
                                {{ product.state }}
                                <br />{{ product.city }} <br />{{
                                    product.street
                                }}
                            </div>
                        </div>
                    </div>
                    <div class="h-14 d-flex justify-content-end border-top border-gray-400 py-2 mt-2">
                        <div class="row gap-3">
                            <router-link :to="`/products/${product.id}`"
                                class="p-2 col border btn btn-primary">View</router-link>
                            <router-link :to="`/products/${product.id}/edit`"
                                class="p-2 col border btn btn-success">Edit</router-link>
                            <button @click="deleteProduct(product.id)" type="button"
                                class="p-2 col border btn btn-danger">Delete</button>
                        </div>

                    </div>
                </div>
                <!-- <td>{{ product.id }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.price }}</td>
                <img :src="product.thumbnail" :alt="product.name">
                <td>
                    <div class="row gap-3">
                    <router-link :to="`/products/${product.id}`" class="p-2 col border btn btn-primary">View</router-link>
                    <router-link :to="`/products/${product.id}/edit`" class="p-2 col border btn btn-success">Edit</router-link>
                    <button @click="deleteProduct(product.id)" type="button" class="p-2 col border btn btn-danger">Delete</button>
                    </div>
                </td> -->
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            products: []
        }
    },
    async created() {
        try {
            const response = await axios.get('/api/products');
            this.products = response.data;
        } catch (error) {
            console.error(error);
        }
    },
    methods: {
        async deleteProduct(id) {
            try {
                await axios.delete(`/api/products/${id}`);
                this.products = this.products.filter(product => product.id !== id);
            } catch (error) {
                console.error(error);
            }
        }
    }
}
</script>
