<template>
  <div>
    <div class="hello">Testing Kin</div>
    <div>
      <button @click="create">Create</button>
      <pre>{{ step1 | pretty }}</pre>
    </div>
    <div>
      <button @click="resolve">Resolve</button>
      <pre>{{ step2 | pretty }}</pre>
    </div>
    <div>
      <button @click="airdrop">Airdrop</button>
      <pre>{{ step3 | pretty }}</pre>
    </div>
    <div>
      <button @click="sendKin">Send</button>
      <pre>{{ step4 | pretty }}</pre>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/no-non-null-assertion */

import { createWallet, KinClient, KinTest, Wallet } from '@kin-sdk/client'
import Vue from 'vue'

const client = new KinClient(KinTest)
const account: Wallet = createWallet('create', { name: 'Account 1' })

const destination = 'Don8L4DTVrUrRAcVTsFoCRqei5Mokde3CV3K9Ut4nAGZ'

export default Vue.extend({
  name: 'HelloWorld',
  methods: {
    async create() {
      const [result, error] = await client.createAccount(account.secret!)
      if (error) {
        console.error(error)
      } else {
        console.log(result)
        this.step1 = result;
      }
    },
    async resolve() {
      const [result, error] = await client.getBalances(account.publicKey!)
      if (error) {
        console.error(error)
      } else {
        console.log(result)
        this.step2 = result;
      }
    },
    async airdrop() {
      const [result, error] = await client.requestAirdrop(account.publicKey!, '1000')
      if (error) {
        console.error(error)
      } else {
        console.log(result)
        this.step3 = result;
      }
    },
    async sendKin() {
      const [result, error] = await client.submitPayment({
        secret: account.secret!,
        tokenAccount: account.publicKey!,
        amount: '10',
        destination,
        memo: 'One Kin for Donald',
      })
      if (error) {
        console.error(error)
      } else {
        console.log(result)
        this.step4 = result;
      }

    },
  },
  data() {
    return {
      step1: {},
      step2: {},
      step3: {},
      step4: {},
    };
  },
   filters: {
    pretty: function(value : any) {
      if(!Object.keys(value).length) return "";
      return JSON.stringify(value, null, 2);
    }
  },
});
</script>
