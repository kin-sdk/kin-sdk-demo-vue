<template>
  <div>
    <div class="hello">Testing Kin</div>
    <button @click="resolve">Resolve</button>
    <button @click="create">Create</button>
    <button @click="sendKin">Send</button>
  </div>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/no-non-null-assertion */

import { KinClient, KinProd, Wallet } from '@kin-sdk/client'
import Vue from 'vue'

const client = new KinClient(KinProd);
const account: Wallet = KinClient.createWallet("create", { name: "Account 1" });


const destination = "Don8L4DTVrUrRAcVTsFoCRqei5Mokde3CV3K9Ut4nAGZ";

export default Vue.extend({
  name: "HelloWorld",
  methods: {
    async sendKin() {
      const response = await client.submitPayment({
        secret: account.secret!,
        tokenAccount: account.publicKey!,
        amount: "1",
        destination,
        memo: "One Kin for Donald",
      });
      console.log(response);
    },
    async create() {
      const [result, error] = await client.createAccount(account.secret!);
      if (error) {
        console.error(error);
      } else {
        console.log(result);
      }
    },
    async resolve() {
      const [result, error] = await client.resolveTokenAccounts(account.publicKey!);
      if (error) {
        console.error(error);
      } else {
        console.log(result);
      }
    },
  },
  props: {
    msg: String,
  },
});
</script>
