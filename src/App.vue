<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>

    <button @click="pay()">Pay</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import midtransClient from 'midtrans-client';
// const midtransClient = require('midtrans-client');
// exampple snaptoken="54ccfa1e-a290-4cbb-8c79-26855d38d762"
var t2=''
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  methods: {
    pay: () => {
            window.snap.pay(t2, {
          // Optional
          onSuccess: function(result){
            /* You may add your own js here, this is just example */ document.getElementById('result-json').innerHTML += JSON.stringify(result, null, 2);
          },
          // Optional
          onPending: function(result){
            /* You may add your own js here, this is just example */ document.getElementById('result-json').innerHTML += JSON.stringify(result, null, 2);
          },
          // Optional
          onError: function(result){
            /* You may add your own js here, this is just example */ document.getElementById('result-json').innerHTML += JSON.stringify(result, null, 2);
          }
        });
      // Create Snap API instance, empty config
    

      // You don't have to re-set using all the options, 
      // i.e. set serverKey only
     

      // example parameter
    

   
    }
  },
  mounted(){
  let snap = new midtransClient.Snap();
      
      snap.apiConfig.set({
        isProduction : false,
        serverKey : 'SB-Mid-server-NZEpgt7TfGWe675szo1VTyao',
        clientKey : 'SB-Mid-client-WxSB3m3iWqKxhh8D'
      });
       snap.apiConfig.set({serverKey : 'SB-Mid-server-NZEpgt7TfGWe675szo1VTyao'});
     let parameter = {
        "transaction_details": {
          "order_id": "test-transaction-123",
          "gross_amount": 200000
        }, "credit_card":{ "secure" : true }
      };
         snap.createTransaction(parameter)
        .then((transaction) => {
          // transaction token
         t2= transaction.token;
          console.log('transactionToken:',t2);

          // transaction redirect url
          // let transactionRedirectUrl = transaction.redirect_url;
          // console.log('transactionRedirectUrl:',transactionRedirectUrl);
        })
        .catch((e) => {
          console.log('Error occured:',e.message);
        });
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
