<template>
    <div class="container-fluid">
        <h2>Stock List</h2>
        <div>
            <button class="btn btn-primary mb-3" @click="addNewStock">Add New Stock</button>
            <table class="table table-hover">
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Value</th>
                        <th>Purchase Date</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="stock in stocks" :key="stock.id">
                        <td>{{ stock.name }}</td>
                        <td>{{ stock.value }}</td>
                        <td>{{ stock.purchase_date }}</td>
                        <td>
                            <button @click="editStock(stock)">Edit</button>
                            <button @click="deleteStock(stock)">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import { APIService } from '../http/APIService';
const apiService = new APIService();

export default {
    name: 'StockList',
    data() {
        return {
            stocks: []
        };
    },
    methods: {
        getStocks() {
            apiService.getStockList().then(response => {
                this.stocks = response.data;
            }).catch(error => {
                console.error(error);
            });
        },
        addNewStock() {
            this.$router.push('/stock-create');
        },
        editStock(stock) {
            this.$router.push(`/stock-create/${stock.id}`);
        },
        deleteStock(stock) {
            if (confirm("Are you sure you want to delete this stock?")) {
                apiService.deleteStock(stock.id).then(response => {
                    if (response.status === 204) {
                        this.getStocks();
                    }
                }).catch(error => {
                    console.error(error);
                });
            }
        }
    },
    mounted() {
        this.getStocks();
    }
};
</script>
