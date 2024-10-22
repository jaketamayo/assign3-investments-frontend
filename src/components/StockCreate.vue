<template>
    <div class="container-fluid">
        <div class="row align-items-center justify-content-center">
            <div class="col-12 col-sm-10 col-md-10 col-lg-6">
                <div class="card">
                    <div class="card-header">{{ pageTitle }}</div>
                    <div class="card-body">
                        <form ref="form">
                            <div class="form-group row py-2">
                                <label class="col-4">Stock Name</label>
                                <div class="col-8">
                                    <input v-model="stock.name" type="text" class="form-control">
                                </div>
                            </div>
                            <div class="form-group row py-2">
                                <label class="col-4">Stock Value</label>
                                <div class="col-8">
                                    <input v-model="stock.value" type="number" class="form-control">
                                </div>
                            </div>
                            <div class="form-group row py-2">
                                <label class="col-4">Purchase Date</label>
                                <div class="col-8">
                                    <input v-model="stock.purchase_date" type="date" class="form-control">
                                </div>
                            </div>
                            <div class="row justify-content-around">
                                <button v-if="!isUpdate" type="button" class="btn btn-primary col-4"
                                    @click="createStock">Save</button>
                                <button v-if="isUpdate" type="button" class="btn btn-primary col-4"
                                    @click="updateStock">Update</button>
                                <button type="button" class="btn btn-secondary col-4"
                                    @click="cancelOperation">Cancel</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { APIService } from '../http/APIService';
const apiService = new APIService();

export default {
    name: 'StockCreate',
    data() {
        return {
            stock: {},
            pageTitle: "Add New Stock",
            isUpdate: false,
        };
    },
    methods: {
        createStock() {
            apiService.addNewStock(this.stock).then(response => {
                if (response.status === 201) {
                    this.stock = response.data;
                    this.$router.push('/stock-list');
                }
            }).catch(error => {
                console.error(error);
            });
        },
        updateStock() {
            apiService.updateStock(this.stock).then(response => {
                if (response.status === 200) {
                    this.stock = response.data;
                    this.$router.push('/stock-list');
                }
            }).catch(error => {
                console.error(error);
            });
        },
        cancelOperation() {
            this.$router.push('/stock-list');
        },
    },
    mounted() {
        if (this.$route.params.pk) {
            this.pageTitle = "Edit Stock";
            this.isUpdate = true;
            apiService.getStock(this.$route.params.pk).then(response => {
                this.stock = response.data;
            }).catch(error => {
                console.error(error);
            });
        }
    }
};
</script>
