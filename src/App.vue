<template>
    <div id="app">
        <AppHeader/>
        <b-container>
            <b-row class="justify-content-center">
                <AddProduct @addProduct="addProduct"/>
                <ListProduct :products="productList" @deleteProduct="deleteProduct" @updateProduct="updateProduct"/>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import AppHeader from "./components/AppHeader";
    import AddProduct from "./components/AddProduct";
    import ListProduct from "./components/ListProduct";
    import axios from "axios"

    export default {
        name: 'App',
        components: {AppHeader, AddProduct, ListProduct},
        data() {
            return {
                productList: []
            }
        },
        methods: {
            async getProductList() {
                try {
                    let result = await axios.get('http://localhost:3000/products');
                    this.productList = result.data;
                } catch (e) {
                    console.log('Error', e)
                }
            },
            async addProduct(newProduct) {
                console.log("newProduct", newProduct);
                try {
                    await axios.post('http://localhost:3000/products', newProduct);
                    this.getProductList();
                } catch (e) {
                    console.log('Error', e);
                }
            },
            async deleteProduct(productId) {
                try {
                    await axios.delete(`http://localhost:3000/products/${productId}`);
                    this.getProductList();
                } catch (e) {
                    console.log('Error', e);
                }
            },
            async updateProduct(updatedProduct){
                console.log("updatedProduct",updatedProduct)
                try {
                    await axios.put(`http://localhost:3000/products/${updatedProduct.id}`, updatedProduct);
                    this.getProductList();
                } catch (e) {
                    console.log('Error', e);
                }
            }
        },
        mounted() {
            this.getProductList();
        }
    }
</script>

<style>

</style>
