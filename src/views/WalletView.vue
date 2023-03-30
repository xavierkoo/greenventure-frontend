
<template>
    <div class="h-100 container" >
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
                    <button class=" btn-primary rounded-pill  w-25 text-white m-2 btn " data-bs-toggle="modal" :data-bs-target="'#use-' + myvouchers.voucherID">Use</button>
                    <div :id="'use-' + myvouchers.voucherID" class="modal fade" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
                        <div class="modal-dialog" role="document">
                          <div class="modal-content">
                            <div class="modal-header">
                              <h5 id="exampleModalLabel" class="modal-title text-dark">Use Voucher</h5>
                              <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                              </button>
                            </div>
                            <div class="modal-body text-dark">
                                <div v-if="useResult==''">
                                    Are you sure you want to use the <h6 class="d-inline">{{myvouchers.voucher_code}}</h6> voucher? 
                                </div>
                                <div v-else>
                                    <div class="text-success text-center">{{useResult}}</div>
                                    <img class="text-center" src="src/assets/img/qr.png" alt="">
                                </div>
                            </div>
                            <div class="modal-footer">
                              <button type="button" class="light-button" data-bs-dismiss="modal" @click="useResult=''" >Close</button>
                              <button type="button" class="blue-button" :disabled="useResult!=''"  @click="use(myvouchers.voucher_code,walletdata)" >Use</button>
                            </div>
                          </div>
                        </div>
                    </div>
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
                    <button class=" btn-primary rounded-pill  w-25 text-white m-2 btn" data-bs-toggle="modal" :data-bs-target="'#detail-' + vouchers.voucherID" >See more</button>
                    <div :id="'detail-' + vouchers.voucherID" class="modal fade" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
                        <div class="modal-dialog" role="document">
                          <div class="modal-content">
                            <div class="modal-header">
                              <h5 id="exampleModalLabel" class="modal-title text-dark">{{vouchers.merchantName}}</h5>
                              <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                              </button>
                            </div>
                            <div class="modal-body text-dark">
                                    <img :src="'src/assets/img/' + vouchers.merchantName + '.png'" >
                                    <h4>{{vouchers.voucherName}}</h4>
                                    <h6 class="d-inline-block my-2">Points Required:</h6> {{vouchers.pointsRequired}} <br>
                                    <h6 class="d-inline-block my-2">Quantity Remaining:</h6> {{vouchers.quantity}} <br>
                                    <h6 class="mt-2">Terms & Conditions:</h6>{{vouchers.description}}
                                    <div v-if="walletdata.points<vouchers.pointsRequired?true:false" class="text-danger" >You do not have enough points to redeem this voucher</div>
                                    <div v-if="redemptionResult" class="text-success">{{redemptionResult}}</div>
                            </div>
                            <div class="modal-footer">
                              <button type="button" class="light-button" data-bs-dismiss="modal" @click="redemptionResult=''">Close</button>
                              <button type="button" class="blue-button"  :disabled="(walletdata.points<vouchers.pointsRequired?true:false)|| redemptionResult!=''" @click="redeem(vouchers)" >Redeem</button>
                            </div>
                          </div>
                        </div>
                    </div>
                </li>
            </ol>

        </div>
    </div>

    </div>
</template>



<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

const redemptionResult = ref('')
const useResult = ref('')
const walletdata = ref([]);

// const state = reactive({
//   data: {
//     // properties of your data object
//   },
// });


const state ={
    "id" : "1"
}

function redeem(vouchers) {
    axios.post('http://127.0.0.1:5204/redeemVoucher', {
        "user_ID": 1,
        "voucherID": vouchers.voucherID,
        "voucher_code": vouchers.voucherCode,
        "voucher_amount": vouchers.pointsRequired,
        "email": "ray.quek@gmail.com"
    })
    .then(function (response) {
        redemptionResult.value = response.data.data.returnMessage
    })
    .catch(function (error) {
        console.log(error)
    })

  }

  function use(myvouchers,walletdata) {
    axios.post('http://127.0.0.1:5205/useVoucher', {
        "walletID": walletdata.user_vouchers[0].voucherID.toString(),
        "voucher_code": myvouchers,
        "email": "ray.quek@gmail.com"
    })
    .then(function (response) {
        useResult.value = response.data.data.returnMessage
    })
    .catch(function (error) {
        console.log(error)
    })

  }


const postData = async () => {
  try {
    const response = await axios.post('http://127.0.0.1:5203/get_wallet_details', state);
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
