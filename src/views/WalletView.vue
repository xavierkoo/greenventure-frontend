<template>
    <div class="h-100" >
    <h4> Your available points: {{walletdata.points}}  </h4>  
    <div class="row">
        <div class="col-6">
            <h5>My Vouchers</h5>
            <ol class="list-group list-group-numbered">
                <li v-for="myvouchers in walletdata.user_vouchers" :key="myvouchers.voucherID" class="list-group-item d-flex justify-content-between align-items-start" >
                    <div class="ms-2 me-auto w-75">
                    <div class="fw-bold">{{myvouchers.voucher_code}}</div>
                    {{ myvouchers.voucherID }}
                    </div>
                    <button class=" btn-primary rounded-pill  w-25 text-white m-2 btn">Use</button>
                </li>
                
            </ol>

        </div>
        <div class="col-6">
            <h5>Vouchers shop</h5>
            <ol class="list-group list-group-numbered">
                <li v-for="vouchers in walletdata.available_vouchers" :key="vouchers.voucherID" class="list-group-item d-flex justify-content-between align-items-start" >
                    <div class="ms-2 me-auto">
                    <div class="fw-bold">{{vouchers.voucherCode}}</div>
                    Points required {{ vouchers.pointsRequired }}
                    </div>
                    <button class=" btn-primary rounded-pill  w-25 text-white m-2 btn">See more</button>
                </li>
            </ol>

        </div>
    </div>

    </div>
</template>

<script setup>

import { onMounted, ref } from 'vue';
import axios from 'axios';
const walletdata = ref([]);

// const state = reactive({
//   data: {
//     // properties of your data object
//   },
// });

const state ={
    "id" : "1"
}

const postData = async () => {
  try {
    const response = await axios.post('http://127.0.0.1:5107/get_wallet_details', state);
    walletdata.value = response.data
    // handle the response if needed
  } catch (error) {
    // handle the error if needed
  }
};

onMounted(() => {
  postData();
});

</script>