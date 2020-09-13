<template>
    <div>
      <nav-header></nav-header>
      <div class="container">
        <div class="page-title-normal">
          <h2 class="page-title-h2"><span>check out</span></h2>
        </div>
        <!-- 进度条 -->
        <div class="check-step">
          <ul>
            <li class="cur"><span>Confirm</span> address</li>
            <li class="cur"><span>View your</span> order</li>
            <li class="cur"><span>Make</span> payment</li>
            <li class="cur"><span>Order</span> confirmation</li>
          </ul>
        </div>

        <div class="order-create">
          <div class="order-create-pic"><img src="/static/ok-2.png" alt=""></div>
          <div class="order-create-main">
            <h3>恭喜！<br>你的订单完成</h3>
            <p>
              <span>订单号：{{orderId}}</span>
              <span>付块金额：{{orderTotal|currency('$')}}</span>
            </p>
            <div class="order-create-btn-wrap">
              <div class="btn-l-wrap">
                <router-link class="btn btn--m" to="/cart">购物车列表</router-link>
              </div>
              <div class="btn-r-wrap">
                <router-link class="btn btn--m" to="/">商品列表</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
      <nav-footer></nav-footer>
    </div>
</template>
<script>
    import NavHeader from './../components/Header'
    import NavFooter from './../components/Footer'
    import NavBread from './../components/Bread'
    import {currency} from './../util/currency'
    import axios from 'axios'
    export default{
        data(){
            return{
                orderId:'',
                orderTotal:0
            }
        },
        components:{
          NavHeader,
          NavFooter,
          NavBread
        },
        filters:{
          currency:currency
        },
        mounted(){
            var orderId = this.$route.query.orderId;
            console.log("orderId:"+orderId);
            if(!orderId){
              return;
            }
            axios.get("/users/orderDetail",{
                params:{
                  orderId:orderId
                }
            }).then((response)=>{
                let res = response.data;
                if(res.status=='0'){
                    this.orderId = orderId;
                    this.orderTotal = res.result.orderTotal;
                }
            });
        }
    }
</script>
