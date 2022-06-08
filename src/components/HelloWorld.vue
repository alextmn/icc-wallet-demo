<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div v-if="step === 0">
      <h2>Step 1: Generate Delithium ICC Key Pair</h2>

    </div>
    <div v-if="step === 1">
      <h2>Step 2: Create Transaction</h2>
          <hr><br /><br />
      <span><b>Wallet Delithium Secret Key</b></span><br />
      <textarea v-model="sk" style="width: 900px; height: 100px;"></textarea><br />
      <span><b>Wallet Delithium Public Key</b></span><br />
      <textarea v-model="sk" style="width: 900px; height: 100px;"></textarea><br />
    </div>
    <div v-if="step === 2">
      <h2>Step 3: Create Transaction and Sign</h2>
      <span><b>ICC Destination Address</b></span><br />
      <input v-model="sha256" style="width: 900px;" /><br />
      <span><b>Value</b></span><br />
      <input value="100" style="width: 900px;" /><br /><br />
    </div>
    <div v-if="step === 3">
      <h2>Step 4: Verify ICC Transaction by Validators</h2>
      <span><b>ICC Wallet Transaction Signature</b></span><br />
      <textarea v-model="sk" style="width: 900px; height: 100px;"></textarea><br />
    </div>
    <div v-if="step === 4">
      <h2>Step 4: Put Transaction on Solana</h2>
      <span><b>ICC Validator Signature</b></span><br />
      <input v-model="sha256" style="width: 900px;" /><br />
    </div>
     <div v-if="step === 5">
      <h2>Step 5: Solana output</h2>
      <span><b>TODO: print solana confirmation</b></span><br />
    </div>
  </div>
  <span v-if="isExecuting">Wait....</span>
  <button @click="next()" v-if="!isExecuting && step < 5">
    Next >
  </button>
</template>

<script>
export default {
  name: 'ICC Demo',
  data() {
    return {
      step: 0,
      isExecuting: false,
    }
  },
  props: {
    msg: String
  },
  methods: {
    makeid: function (length) {
      let result = '';
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      const charactersLength = characters.length;
      for (var i = 0; i < length; i++) {
        result += characters.charAt(Math.floor(Math.random() *
          charactersLength));
      }
      return result;
    },
    next: function () {
      this.sk = this.makeid(1024)
      this.sha256 = this.makeid(1024)
      this.isExecuting = true;
      setTimeout(() => {
        this.isExecuting = false;
        this.step++;
      }, 1000);

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
