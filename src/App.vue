<template>
  <div id="app">
    <div class="container">
      <!--<Header></Header>-->
      <!--<router-view name="header-top"/>-->
      <!--<router-view/>-->
      <!--<router-view name="header-bottom"/>-->
      <template v-if="Object.entries(first).length !== 0">
        <h2>First Request</h2>
        <ul class="row">
          <li class="col-3 text-left" v-for="curr in first.data">
            <p>CCY: {{ curr.ccy }}</p>
            <p>Base: {{ curr.base_ccy }}</p>
            <p>Buy: {{ curr.buy }}</p>
            <p>Sale: {{ curr.sale }}</p>
          </li>
        </ul>
      </template>
      <template v-if="Object.entries(second).length !== 0">
        <hr>
        <h2>Second Request</h2>
        <ul class="row">
          <li class="col-3 text-left" v-for="curr in second.data">
            <p>CCY: {{ curr.ccy }}</p>
            <p>Base: {{ curr.base_ccy }}</p>
            <p>Buy: {{ curr.buy }}</p>
            <p>Sale: {{ curr.sale }}</p>
          </li>
        </ul>
      </template>
      <hr>
      <template v-if="Object.entries(oneOf).length !== 0">
        <h2>One of Requests</h2>
        <ul class="row">
          <li class="col-3 text-left" v-for="curr in oneOf">
            <p>CCY: {{ curr.ccy }}</p>
            <p>Base: {{ curr.base_ccy }}</p>
            <p>Buy: {{ curr.buy }}</p>
            <p>Sale: {{ curr.sale }}</p>
          </li>
        </ul>
      </template>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
import Header from './components/Header'

export default {
  name: 'App',
  components: {
    Header
  },
  data () {
    return {
      first: {},
      second: {},
      oneOf: {},

      // url1: 'https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5',
      // url2: 'https://api.privatbank.ua/p24api/pboffice?city=Ровно',
      // url3: ' https://api.privatbank.ua/p24api/exchange_rates?json&date=01.12.2014',
    }
  },
  methods: {
    statusCheck(status, index='') {
      if (status < 200 && status >= 400) {
      // if (true) {
        alert(`${index>=0 ? index+1 : 'Oooops'} request crashed`);
        throw 'crashed';
      }
    }
  },
  mounted () {
    const p1 = axios.get('https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5');
    const p2 = axios.get('https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=3');

  /* One by One  ---- start */
    p1
      .then(resp => {
        this.statusCheck(resp.status);
        self.first = resp;
      })
      .then(() => {
        return p2;
      })
      .then(secResp => {
        this.statusCheck(secResp.status);
        self.second = secResp;
      })
      .catch(err => {
        console.log(err);
      })
    /* One by One  ---- end */

  /* Promise.all  ---- start */
    // Promise.all([p1, p2])
    //   .then(resp => {
    //     for (const index of resp.keys()) {
    //       this.statusCheck(resp[index].status, index);
    //     }
    //     [this.first, this.second] = resp;
    //   })
    //   .catch(err => {
    //     console.log('Crashed ------>', err);
    //   });
  /* Promise.all  ---- end */

  /* Promise.race  ---- start */
    // Promise.race([p1, p2])
    //   .then(resp => {
    //     this.statusCheck(resp.status);
    //     this.oneOf = resp.data;
    //   })
    //   .catch(err => {
    //     console.log('Crashed ------>', err);
    //   });
  /* Promise.all  ---- end */

  }
}
</script>

<style></style>
