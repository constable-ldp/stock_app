<template>
  <div>
    <h1>Stocks</h1>
    <h2>Type in the stock symbol:</h2>
    <p v-if="symbolWrong">{{this.symbolWrong}}</p>
    <stock-search :today="today" />
    <stock-graph :stockGraphData="stockGraphData" />

  </div>
</template>

<script>
import StockSearch from './components/StockSearch.vue'
import StockGraph from './components/StockGraph.vue'
import { eventBus } from './main.js'


export default {
  name:'App',

  data(){
    return {
      apiKey: '',
      symbol: 'null',
      // dateFrom: ['2021-03-01','2021-04-01'],
      dates: [],
      url: '',
      stockData: [],
      'today': new Date().toISOString().split("T")[0],
      stockDates: [],
      symbolWrong: '',
      stockGraphData: [["Date", "Value"]]
    }
  },

  components: {
    'stock-search': StockSearch,
    'stock-graph' : StockGraph
    
  },

  mounted(){
    eventBus.$on('stock-selected', (stock) => {
      this.dates = []
      this.symbol = stock.symbol
      this.dates.push(stock.startDate)
      if (stock.endDate !== ''){
        this.dates.push(stock.endDate)
      }
      this.getData()
      })
    
  },

  methods: {

    getUrl: function() {
      const pageLength = this.dates.length
      if (pageLength === 1){
         this.url = `http://api.marketstack.com/v1/eod?access_key=${this.apiKey}&symbols=${this.symbol}&date_from=${this.dates[0]}`
      }
      else {
        this.url = `http://api.marketstack.com/v1/eod?access_key=${this.apiKey}&symbols=${this.symbol}&date_from=${this.dates[0]}&date_to=${this.dates[1]}`
      }
    },

    getData: async function() {
      this.stockData = []
      this.stockDates = []
      this.symbolWrong = ''
      this.getUrl()
      try {
        const response = await fetch(this.url)
        const data = await response.json()
        if (data.data === []){
          this.symbolWrong = 'This symbol does not match any symbol in the API, please enter another symbol'
        }
        else {
          const values = data.data.map(stockData => stockData.close)
          let dates = data.data.map(stockData => stockData.date)
          dates = dates.map(element => element.split("T")[0])
          this.stockData.push(...values.reverse())
          this.stockDates.push(...dates.reverse())
          for (let i=0; i<=this.stockData.length-1; i++) {
            this.stockGraphData.push([this.stockDates[i],this.stockData[i]])
            }
          }
        }
      catch {
        this.symbolWrong = 'This symbol does not match any symbol in the API, please enter another symbol'
        }
      }
    }

    // getUrls: function() {
    //   const pageLength = this.dateFrom.length
    //   this.urls.push(`http://api.marketstack.com/v1/eod?access_key=${this.apiKey}&symbols=${this.symbol}&date_from=${this.dateFrom[0]}`)
    //   if (pageLength > 1) {
    //   //   for (let i=1; i<=pageLength-1; i++) {
    //       this.urls.push((`http://api.marketstack.com/v1/eod?access_key=${this.apiKey}&symbols=${this.symbol}&date_from=${this.dateFrom[i-1]}&date_to=${this.dateFrom[i]}`))
    //   //   }
    //   }
    // },

    // getData: async function() {
    //   this.getUrls()
    //   const responses = await Promise.all(this.urls.map(url => fetch(url)))
    //   const data = await Promise.all(responses.map(res => res.json()))
    //   // Formatting the data into a useable array using multiple flatmaps
    //   let values = data.flatMap(stockData => stockData.data)
    //   values = values.flatMap(stockData => stockData.close)
    //   this.stockData.push(...values)
    // }
  // }

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
