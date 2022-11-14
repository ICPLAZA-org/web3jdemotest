<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h3>Address{{ address }}</h3>
    <input v-model="message" placeholder="Receiving address">
    <ul>
      <li>
        <button
          class="pup-btu"
          @click="sendContract()">Send contract</button>
      </li>
      <br>
      <br>
      <li>
        <button
          class="pup-btu"
          @click="authorization()">Apply for authorization</button>
      </li>
      <br>
      <br>
      <li>
        <button
          class="pup-btu"
          @click="isAuth()">Whether authorized</button>
      </li>
      <br>
      <br>
      <li>
        <button
          class="pup-btu"
          @click="getContractBalanceOf()">Query contract balance</button>
      </li>
      <br>
      <br>
      <li>
        <button
          class="pup-btu"
          @click="SendICT()">Ordinary ict transaction</button>
      </li>
    </ul>
    <h2>Related documents</h2>
    <ul>
      <li>
        <a
          href="https://web3.tryblockchain.org/Web3.js-api-refrence.html#toc_49"
          target="_blank"
        >
          Web3j document
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import Web from 'web3'
import erc20 from '../json/erc20.json'
export default {
  name: 'Home',
  data () {
    return {
      msg: 'Welcome',
      address: '',
      message: '0x9f6fB5D344f4231Ab344A228272774677BEc81FC'
    }
  },
  mounted () {
    this.getwallet()
  },
  methods: {
    async getwallet () {
      // If the web3modal used for connection uses a standard connection, it can
      // If you use walletconnect, you need to connect in the walletconnection mode
      var web3Provider
      if (window.ethereum) {
        web3Provider = window.ethereum
        let web3 = new Web(web3Provider)

        // Use icplaza. This is a necessary condition for initialization of the requested address
        web3.eth.requestAccounts().then(res => (this.address = res[0]))

        const accounts = await web3.eth.getAccounts()
        console.log('accounts', accounts[0])
      }
    },
    async sendContract () {
      let web3 = new Web(window.ethereum)
      const accounts = await web3.eth.getAccounts()
      // Use for contract transfer transaction
      const myContract = new web3.eth.Contract(
        erc20.abi,
        // In case of transfer, use your own address; in case of purchase by contract, use the contract address
        '0xdd55771023f4848bb815957b037588e2eeed5c3b'
      )
      myContract.methods
        .transfer(this.message, 2000000000000)
        .send({ from: accounts[0], value: 2000000000000 })
        .then(res => {
          alert(res)
        },
        err => {
          alert(err)
        }
        )
    },
    // 0x9f6fB5D344f4231Ab344A228272774677BEc81FC
    // erc20 authorization
    async authorization () {
      let web3 = new Web(window.ethereum)
      var account = await web3.eth.getAccounts()
      var accounts = account[0]
      // 0x334A4801C21Bc279a63cB80E703E4da704057C13 is Contract address
      var contract = new web3.eth.Contract(erc20.abi, this.message)
      var price = '1000000000000000000'
      contract.methods
        .approve(this.message, price)
        .send({
          from: accounts
        })
        .on('receipt', function (receipt) {
          if (receipt) {
            alert(receipt)
            console.log(receipt, 'Authorization success')
          }
        })
        .on('error', function (error) {
          alert(error)
          console.log(error, 'error')
        })
    },
    // Whether authorized
    async isAuth () {
      let web3 = new Web(window.ethereum)
      var account = await web3.eth.getAccounts()
      var accounts = account[0]
      var contract = new web3.eth.Contract(erc20.abi, this.message)

      contract.methods
        .allowance(accounts, this.message)
        .call({ form: accounts }, function (error, res) {
          if (res === 0) {
          } else if (res > 0) {
            alert(res)
          }
          if (error) {
            alert(error)
          }
        })
    },
    async getContractBalanceOf () {
      let web3 = new Web(window.ethereum)
      var account = await web3.eth.getAccounts()
      var accounts = account[0]
      var contract = new web3.eth.Contract(erc20.abi, this.message)
      contract.methods
        .balanceOf(accounts)
        .call({form: accounts}, function (error, res) {
          if (res) {
            alert(res)
          }
          if (error) {
            alert(error)
          }
        })
    },
    async SendU () {
      let web3 = new Web(window.ethereum)
      var account = await web3.eth.getAccounts()
      var accounts = account[0]
      web3.eth.sendTransaction()
      var contract = new web3.eth.Contract(erc20.abi, this.message)
      contract.methods
        .balanceOf(accounts)
        .call({form: accounts}, function (error, res) {
          if (res) {
            alert(res)
          }
          if (error) {
            alert(error)
          }
        })
    },
    async SendICT () {
      let web3 = new Web(window.ethereum)
      var account = await web3.eth.getAccounts()
      var accounts = account[0]
      var getGasPrice = await web3.eth.getGasPrice()
      getGasPrice = Number(getGasPrice)
      var oldGasPrice = await web3.eth.estimateGas({
        gas: getGasPrice
      })
      console.log('gasprice===', oldGasPrice)

      console.log('accounts===', accounts)
      web3.eth.sendTransaction({
        from: account,
        to: '0xdd55771023f4848bb815957b037588e2eeed5c3b',
        value: '2000000000000000000',
        gas: oldGasPrice
      })
        .then(res => {
          alert(res)
        }, err => {
          alert(err)
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
