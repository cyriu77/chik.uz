<template>
    <div class="container">
        <br>
        <div class="order">
	<h1 class="h3 mb-3">Заказ</h1>
<ul class="list-group">
  <li class="list-group-item">Заказ ID: {{  }}</li>
  <li class="list-group-item">Ползователь ID: {{  }}</li>
  <li class="list-group-item">Chik ID: {{  }}</li>
  <li class="list-group-item">Статус: {{  }}</li>
  <li class="list-group-item">Цена: {{  }}</li>
  <li class="list-group-item">Сделано: {{  }}</li>
</ul>
 </div> 
 <br>
 <button type="button" class="btn btn-primary btn-sm">Открыть Заказ</button>
    </div>
    
</template>
<script>
import axios from "axios";

export default {
    emits: ["updateSidebar"],
    data() {
        return {
            orders: [],
        };
    },
    methods: {
        fetchOrders() {
            axios
                .get("/api/orders")
                .then((response) => {
                    this.orders = response.data.data;
                })
                .catch((error) => {
                    console.error("Error fetching orders:", error);
                });
        },
        cancelPurchase(orderId) {
            // Implement cancellation logic as needed
            // Example:
            axios
                .post(`/api/cancel-order/${orderId}`)
                .then(() => {
                    this.fetchOrders(); // Refresh orders after cancellation
                    // Update the total price if needed
                    // this.totalPrice -= postPrice;
                })
                .catch((error) => {
                    console.error("Cancellation failed:", error);
                });
        },
    },
    mounted() {
        this.fetchOrders();
    },
};
</script>

<style scoped>
.btn{
    background-color: #e4606d;
    border-color: #e4606d;
}
</style>