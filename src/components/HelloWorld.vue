<template>
  <div class="hello">
    <div v-if="step === 0">
      <h2>Step #1: Generate IRONCAP Keypair (i.e. Quantum-Safe Wallet)</h2>
      <div style="height:400px" />
    </div>
    <div v-if="step === 1">
      <h2>Step #2: Create Transaction (Smart Contract)</h2>
      <hr />
      <br /><br />
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="sk" class="ta"></textarea>
        <span><b>Wallet ICC Secret Key</b></span>
        <div>&nbsp;</div>
      </div>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="pk" class="ta"></textarea>
        <span
          ><b
            >Wallet ICC Public Key<br />
            (wallet address)</b
          ></span
        >
        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 2">
      <h2>Step #3: Create and Sign the Transaction</h2>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="sha256" class="ta" />
        <span><b>Destination Wallet Address</b></span>
        <div>&nbsp;</div>
      </div>
      <div class="ll">
        <div>&nbsp;</div>
        <div>&nbsp;</div>
        <input value="100"  />
        <div>&nbsp;</div>
        <span><b>Value</b></span>
        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 3">
      <h2>Step #4: Verify ICC Transaction by Validators</h2>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="sk" class="ta"></textarea>
        <span><b>ICC Wallet Transaction Signature</b></span>

        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 4">
      <h2>Step #5: Put Transaction on Solana</h2>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="pk" style="width: 900px; height: 200px" />
        <span><b>ICC Validator Signature</b></span>
        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 5">
      <h2>Step #6: Solana output</h2>
      <span><b>TODO: print solana confirmation</b></span>
    </div>
  </div>
  <div style="display: flex; justify-content: space-between">
    <div>&nbsp;</div>
    <span class="nextBtn" v-if="isExecuting">Wait....</span>
    <button class="nextBtn" @click="next()" v-if="!isExecuting && step < 5">
      Next >
    </button>

    <img
      src="../assets/logo.png"
      style="wight: 100px; height: 100px; align-self: flex-end"
    />
  </div>
</template>

<script>
export default {
  name: "ICC Demo",
  data() {
    return {
      step: 0,
      isExecuting: false,
    };
  },
  props: {
    msg: String,
  },
  methods: {
    makeid: function (length) {
      let result = "";
      const characters =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
      const charactersLength = characters.length;
      for (var i = 0; i < length; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * charactersLength)
        );
      }
      return result;
    },
    next: function () {
      this.sk = this.makeid(1024);
      this.pk = this.makeid(1524);
      this.sha256 = this.makeid(128);
      this.isExecuting = true;
      setTimeout(() => {
        this.isExecuting = false;
        this.step++;
      }, 1000);
    },
  },
};
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

.nextBtn {
  min-height: 100px;
  min-width: 100px;
  font-size: 40px;
   margin: 20px;
}
.ll {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 10px;
}

.ta {
  min-width: 700px;
  min-height: 200px;
}
h2 {
  font-size: 20px;
  margin-left: -100px;
}
</style>
