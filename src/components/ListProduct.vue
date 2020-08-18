<template>
    <b-col md="4" class="m-3">
        <b-card header="List Product">
            <b-card-group deck>
                <b-card
                        style="min-width: 50%"
                        class="m-3"
                        :border-variant="product.inventoryStatus? 'success': 'danger'"
                        align="center"
                        v-for="product in products" :key="product.id"
                >
                    <b-badge class="mb-3" :variant="product.inventoryStatus? 'success': 'danger'">{{product.inventoryStatus? 'IN STOCK': 'OUR OF STOCK'}}</b-badge>
                    <b-card-text><strong>Name: </strong>{{product.name}}</b-card-text>
                    <b-card-text><strong>Price: </strong>$ {{product.price}}</b-card-text>
                    <b-card-text><strong>Brand: </strong>{{product.brand}}</b-card-text>
                    <hr/>
                    <b-row>
                        <b-col>
                            <b-button variant="danger" @click="deleteProduct(product.id)"><i class="fas fa-trash"></i></b-button>
                        </b-col>
                        <b-col>
                            <UpdateProduct :product="product" @updateProduct="updateProduct"/>
                        </b-col>
                    </b-row>
                </b-card>
            </b-card-group>
        </b-card>
    </b-col>
</template>

<script>
    import UpdateProduct from "./UpdateProduct";
    export default {
        name: "ListProduct",
        components: {UpdateProduct},
        props: ['products'],
        component:{
            UpdateProduct
        },
        methods:{
            deleteProduct(productId){
                this.$emit('deleteProduct', productId)
            },
            updateProduct(updatedProduct){
                this.$emit('updateProduct',updatedProduct)
            }
        }
    }
</script>

<style scoped>

</style>