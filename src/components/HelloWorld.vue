<template>
  <div class="hello">
    <div v-if="step === 0">
      <h2>Step #1: Generate IRONCAP Keypair (i.e. Quantum-Safe Wallet)</h2>
     <div style="height:400px" />
    </div>
    <div v-if="step === 1">
      <h2 style="margin-left: -140px;">Step #2: Create Transaction (Smart Contract)</h2>
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
      <h2 style="margin-left: -140px;">Step #3: Create and Sign the Transaction</h2>
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
      <h2 style="margin-left: -140px;">Step #4: Verify ICC Transaction by Validators</h2>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="iccSignature" class="ta"></textarea>
        <span><b>ICC Wallet Transaction Signature</b></span>

        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 4">
      <h2>Step #5: Put Transaction on Solana</h2>
      <div class="ll">
        <div>&nbsp;</div>
        <textarea v-model="iccValidatorResponse" style="width: 900px; height: 200px" />
        <span><b>ICC Validator Signature</b></span>
        <div>&nbsp;</div>
      </div>
    </div>
    <div v-if="step === 5">
      <h2 style="margin-left: -140px;">Step #6: Solana transaction</h2>
      <span style="font-size:12px;"><b>{{solTx}}</b></span>
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
import axios from 'axios';

export default {
  name: "ICC Demo",
  data() {
    return {
      step: 0,
      isExecuting: false,
      mainUrl: 'http://54.174.187.1:3000',
      dstAddress: 'vtYZ4PebDAe6TxFG7QaqxlghZQaZhTOVi3QOyjgyRKA=',
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
    next: async function () {
      // this.sk = this.makeid(1024);
      // this.pk = this.makeid(1524);
      // this.sha256 = this.makeid(128);
      this.isExecuting = true;
      if (this.step === 0)  {
        await this.keyPair();
      } if (this.step === 2) {
        await this.iccSign();
      } if (this.step === 3) {
        await this.iccValidatorSign();
      } if (this.step === 4) {
        await this.solanaTx();
      }
      
      this.isExecuting = false;
      this.step++;
      // setTimeout(() => {
      //   this.isExecuting = false;
      //   this.step++;
      // }, 1000);
    },

    keyPair: async function () {
      try {
        const url = `${this.mainUrl}/keypair`;
        console.log('executing', url)
        const { data } = await axios.post(`${this.mainUrl}/keypair`)
        console.log(data)
        this.sk = data.sk; 
        this.pk = data.pk; 
        this.sha256 = data.pkHash; 
      } catch (e) {
        console.log(e)
        alert("Cound not generate key pair, error:" + e)
      }
    },

    iccSign: async function () {
      this.signMsg = 
        JSON.stringify({pk:this.sha256, anount: 100, address: this.dstAddress })
      console.log('message to sign',this.signMsg)
      const { data } = await axios.post(`${this.mainUrl}/wallet-sign`, {
        msg:this.signMsg,
        sk: this.sk,
     })
      console.log('response',data)
      this.iccSignature=data.iccSignature;
      this.walletSignature=data.walletSignature;
    },
    iccValidatorSign: async function () {

      console.log('icc validator message to sign',this.signMsg)
      const { data } = await axios.post(`${this.mainUrl}/validator-sign`, {
        msg:this.signMsg,
        msgSigned: this.walletSignature,
        pkHash:this.sha256
        
     })
      
      const iccValidatorSignature=data.validatorResponse;
      iccValidatorSignature.to_account = this.dstAddress;
      iccValidatorSignature.value = 100;
      console.log('iccValidatorSignature',iccValidatorSignature)
      this.iccValidatorResponse= JSON.stringify(iccValidatorSignature);
    },
    solanaTx: async function() {
      const { data } = await axios.post(`${this.mainUrl}/solana-tx`, this.iccValidatorResponse)
      this.solTx = data;
      console.log('solana tx:',data)
    }
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
   padding-left:20px;
   padding-right:20px;
   margin-left: -100px;
  
}
</style>
