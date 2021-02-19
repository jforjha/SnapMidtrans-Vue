<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>

    <button @click="pay()">Pay</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
const midtransClient = require('midtrans-client');

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  methods: {
    pay: () => {
      // Create Snap API instance, empty config
      let snap = new midtransClient.Snap();
      
      snap.apiConfig.set({
        isProduction : false,
        serverKey : 'YOUR_SERVER_KEY',
        clientKey : 'YOUR_CLIENT_KEY'
      });

      // You don't have to re-set using all the options, 
      // i.e. set serverKey only
      snap.apiConfig.set({serverKey : 'YOUR_SERVER_KEY'});
      console.log(snap);

      // example parameter
      let parameter = {
        "transaction_details": {
          "order_id": "test-transaction-123",
          "gross_amount": 200000
        }, "credit_card":{ "secure" : true }
      };

      snap.createTransaction(parameter)
        .then((transaction) => {
          // transaction token
          let transactionToken = transaction.token;
          console.log('transactionToken:',transactionToken);

          // transaction redirect url
          let transactionRedirectUrl = transaction.redirect_url;
          console.log('transactionRedirectUrl:',transactionRedirectUrl);
        })
        .catch((e) => {
          console.log('Error occured:',e.message);
        });
    }
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
