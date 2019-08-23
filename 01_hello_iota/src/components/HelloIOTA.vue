<template>
  <div class="hello">
    <h1>Iri status by <a href="https://github.com/iotaledger/iota.js/#getting-started" target="_blank">getNodeInfo()</a></h1>
    <pre>{{ info }}</pre>
  </div>
</template>

<script>
import { composeAPI } from "@iota/core";

const iota = composeAPI({
  provider: "https://nodes.devnet.thetangle.org:443"
});

export default {
  name: "HelloIOTA",
  data() {
    return {
      info: null
    };
  },
  mounted() {
    iota
      .getNodeInfo()
      .then(info => {
        this.info = info;
        console.log(info);
      })
      .catch(error => {
        console.log(`Request error: ${error.message}`);
      });
  }
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
pre {
	width: 100%;
	overflow: auto;
	overflow-y: hidden;
	font-size: 12px;
	line-height: 20px;
	background: #efefef;
}
pre code {
	padding: 10px;
	color: #333;
}
</style>
