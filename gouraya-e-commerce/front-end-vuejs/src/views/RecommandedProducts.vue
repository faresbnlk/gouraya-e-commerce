<template>
    <div class="page-rec-products">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Recommended Products</h1>

                <h2 class="is-size-5 has-text-grey">Products for you !</h2>
            </div>

            <ProductBox
                v-for="product in products"
                v-bind:key="product.id"
                v-bind:product="product" />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox.vue'

export default {
    name: 'RecommandedProducts',
    components: {
        ProductBox
    },
    data() {
        return {
            products: [],
            query: ''
        }
    },
    mounted() {
        document.title = 'Recommanded Products | Gouraya E-commerce'

        this.getRecProducts()
    },
    methods: {
        async getRecProducts() {
            this.$store.commit('setIsLoading', true)

            await axios
                .post('/api/v1/products/rec-products/', {'query': localStorage.getItem("searchedProducts")})
                .then(response => {
                    this.products = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>
