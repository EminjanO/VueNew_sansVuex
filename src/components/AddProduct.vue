<template>
    <b-col md="4" class="m-3">
        <ValidationObserver ref="observer" v-slot="{ handleSubmit }">

            <b-card
                    header="Add Product"
            >
                <b-card-body>
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
                </b-card-body>
                <b-button block type="submit" variant="primary" @click="handleSubmit(addProduct)">Add Product</b-button>
                <b-button block @click="resetForm">Reset</b-button>
            </b-card>
        </ValidationObserver>

    </b-col>
</template>

<script>
    import {extend} from 'vee-validate';

    // extend('even', value => {
    //     return value % 2 === 0;
    // });
    extend("decimal", {
        validate: (value, {decimals = '*', separator = '.'} = {}) => {
            if (value === null || value === undefined || value === '') {
                return {
                    valid: false
                };
            }
            if (Number(decimals) === 0) {
                return {
                    valid: /^-?\d*$/.test(value),
                };
            }
            const regexPart = decimals === '*' ? '+' : `{1,${decimals}}`;
            const regex = new RegExp(`^[-+]?\\d*(\\${separator}\\d${regexPart})?([eE]{1}[-]?\\d+)?$`);

            return {
                valid: regex.test(value),
            };
        },
        message: 'The {_field_} field must contain only decimal values'
    })

    export default {
        name: "AddProduct",
        data() {
            return {
                form: {
                    name: '',
                    price: '',
                    brand: '',
                    inventoryStatus: ''
                }
            }
        },
        methods: {
            async addProduct() {
                let result = await this.$refs.observer.validate();
                if (result) {
                    this.$emit('addProduct', {
                        name: this.form.name,
                        price: this.form.price,
                        brand: this.form.brand,
                        inventoryStatus: this.form.inventoryStatus === 'true'
                    });
                    this.form = {
                        name: '',
                        price: '',
                        brand: '',
                        inventoryStatus: ''
                    };
                    this.$refs.observer.reset();
                }
            },
            resetForm() {
                this.form.name = '',
                this.form.price = '',
                this.form.brand = '',
                this.form.inventoryStatus = ''//,
                this.$nextTick(() => {
                    this.$refs.observer.reset();
                });
            }
        }
    }
</script>

<style scoped>

</style>