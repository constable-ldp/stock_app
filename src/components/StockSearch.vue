<template>
  <form v-on:submit.prevent>
      <label for="symbol">Stock Symbol:</label>
      <input type="text" id="symbol" v-model="stockSymbol" placeholder="Search for stock via symbol..." required>
      <label for="from_date">From:</label>
      <input type="date" id="from_date" v-model="stockStartDate"  :max="today" required>
      <label for="to_date">To:</label>
      <input type="date" id="to_date" v-model="stockEndDate" :max="today">
      <input @click=searchForStock type="submit" value="Submit"> 
  </form>
</template>

<script>
import { eventBus } from '../main.js'

export default {
    name: 'stock-search',

    data(){
        return {
            'stockSymbol': '',
            'stockStartDate': '',
            'stockEndDate': '',
        }
    },

    props: ['today'],

    methods: {
        searchForStock(){
            eventBus.$emit('stock-selected', {symbol: this.stockSymbol.toUpperCase(), startDate: this.stockStartDate, endDate: this.stockEndDate})
        },

        // sortDate(){
            // const now = new Date()
            // // now = new Date(now.getFullYear(), now.getMonth(), now.getDate())
            // now.setUTCHours(0)
            // now.setUTCMinutes(0)
            // now.setUTCSeconds(0)
            // now.setUTCMilliseconds(0)
            // const startDate = new Date(this.stockStartDate)
            // let difference = now - startDate
            // // milliseconds to days
            // difference = difference/1000/60/60/24
            // const array = new Array(Math.floor(difference / 100)).fill(100).concat(difference % 100)
            // console.log(array)
            // for (const number of array){
            //     now.setDate(now.getDate()-number)
            // .toISOString().split("T")[0],
            // }
        // }


        }

    }



</script>

<style>

</style>