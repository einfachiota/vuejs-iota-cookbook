<template>
  <div class="hello">
    <h1>Send Transaction with an Message</h1>
    <input type="text" v-model="message" placeholder="Your message">
    <button @click="send">Send</button>
    <p>Just type your message and click the "Send" button to send a transaction into the tangle.</p>
    <p>This demo uses <a href="">prepareTransfers()</a> and <a href="">sendTrytes()</a> to send a transaction with an message. The message will converted into trytes via <a href="">asciiToTrytes()</a></p>
    <h2>Transaction list</h2>
    <ul>
      <li v-for="(tx, index) in txs" :key="index"><a :href="'https://devnet.thetangle.org/transaction/' + tx.hash" target="_blank">{{ tx.hash }}</a></li>
    </ul>
  </div>
</template>

<script>

import { composeAPI } from "@iota/core";
const Converter = require("@iota/converter");

const iota = composeAPI({
  provider: "https://nodes.devnet.thetangle.org:443"
});

export default {
  name: 'HelloWorld',
  data() {
    return {
      message: "Hello IOTA!",
      txs: []
    }
  },
  methods: {
    send() {
      console.log("Send transaction")
      // Dies ist unsere IOTA Empfänger Adresse. An diese Werden wir eine "HELLO WORLD" Nachricht schicken.
      const address =
        'EINFACHIOTAELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWOR99D'

      // Dies ist unser Seed. Mithilfe des Seeds schicken wir die Nachricht.
      const seed =
        'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX'

      // Wir erstellen ein Transfer Objekt. Dieses enthält die Anzahl der IOTA Tokens, eine Empfäger Adresse und ein optionaler Tag.
      // Beachte bitte, das sich unser Transfer Objekt in einen Array befindet - somit wäre es auch möglich, mehrere Transvers gleichzeitig zu versenden. 

      const transfers = [
        {
          value: 0,
          address: address,
          message: Converter.asciiToTrytes(this.message)
        }
      ]
      // Hier bereiten wir alles vor uns senden die Transaktion zum Tangle. Wenn alles klappt hat, bekommt ihr ein Budle Objekt in der Konsole angezeigt.
        iota
          .prepareTransfers(seed, transfers)
          .then(trytes => iota.sendTrytes(trytes, 3, 9))
          .then(bundle => {
            console.log(bundle)
            this.fetchTransactions();
          })
          .catch(err => {
            console.error(err)
          })
    },
    fetchTransactions(){
      // Hiermit bekommen wir alle Transaktionen, die an diese Adresse gesendet wurden.
      iota
        .findTransactionObjects({ addresses: ["EINFACHIOTAELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWOR99D"] })
        .then(txs => {
          console.log(txs)
          this.txs = txs;
        })
        .catch(err => {
          console.error(err)
        })
    }
  },

  created(){
    this.fetchTransactions();
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
