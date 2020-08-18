<template>
    <div>
        <b-button @click="modalShow = !modalShow"><i class="fas fa-edit"></i></b-button>
        <ValidationObserver ref="observer">
            <!--            v-slot="{ handleSubmit }"-->
            <b-modal
                    v-model="modalShow"
                    title="Update Product"
                    @show="showModel"
                    @hidden="resetModal"
                    @ok="handleOk"
            >
                <!--                    @ok="handleSubmit(handleOk)"-->
                <b-form autocomplete="off">
                    <b-form-group
                            label="Name"
                    >
                        <validationProvider name="name" rules="required|min:3" v-slot="{errors}">
                            <b-form-input v-model="form.name" id="name" name="name" trim></b-form-input>
                            <span class="text-danger">{{ errors[0] }}</span>
                        </validationProvider>
                    </b-form-group>
                    <b-form-group
                            label="Price ($)"
                    >
                        <validationProvider name="price" rules="required|decimal" v-slot="{errors}">
                            <b-form-input v-model="form.price" id="price" name="price" trim></b-form-input>
                            <span class="text-danger">{{ errors[0] }}</span>
                        </validationProvider>
                    </b-form-group>
                    <b-form-group
                            label="Brand"
                    >
                        <validationProvider name="brand" rules="required" v-slot="{errors}">
                            <b-form-input v-model="form.brand" id="brand" name="brand" trim></b-form-input>
                            <span class="text-danger">{{ errors[0] }}</span>
                        </validationProvider>
                    </b-form-group>
                    <b-form-group label="Inventory?">
                        <validationProvider name="inventorystatus" rules="required" v-slot="{errors}">
                            <b-form-radio v-model="form.inventoryStatus" name="inventorystatus" value="true">In
                                Stock
                            </b-form-radio>
                            <b-form-radio v-model="form.inventoryStatus" name="inventorystatus" value="false">Out Of
                                Stock
                            </b-form-radio>
                            <span class="text-danger">{{ errors[0] }}</span>
                        </validationProvider>
                    </b-form-group>
                </b-form>
            </b-modal>
        </ValidationObserver>
    </div>
</template>

<script>
    export default {
        name: "UpdateProduct",
        props: ['product'],
        data() {
            return {
                modalShow: false,
                form: {
                    name: '',
                    price: '',
                    brand: '',
                    inventoryStatus: ''
                }
            }
        },
        methods: {
            showModel() {
                console.log("modal open")
                this.form = {
                    name: this.$props.product.name,
                    price: this.$props.product.price,
                    brand: this.$props.product.brand,
                    inventoryStatus: this.$props.product.inventoryStatus.toString()
                }
            },
            resetModal() {
                console.log("modal closed")
                this.form = {}
            },
            async handleOk(bvModalEvt) {
                // Prevent modal from closing
                bvModalEvt.preventDefault()
                console.log("modal Submitted")
                let result = await this.$refs.observer.validate();
                console.log("result", result)
                if (result) {
                    this.$emit('updateProduct', {
                        name: this.form.name,
                        price: this.form.price,
                        brand: this.form.brand,
                        inventoryStatus: this.form.inventoryStatus === 'true' ? true : false,
                        id: this.$props.product.id

                    })
                    this.modalShow = false;
                }
            }
        }
    }
</script>

<style scoped>

</style>