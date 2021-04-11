<template>
    <div>
        <!-- <p>{{this.stockGraphData}}</p> -->
        <GChart
            type="LineChart"
            :data="chartData"
            :options="chartOptions"
        />
    </div>
</template>

<script>
export default {
    name: 'stock-graph',
    props: ['stockGraphData'],

    data () {
    return {
      // Array will be automatically processed with visualization.arrayToDataTable function
      chartData: this.stockGraphData,
      chartOptions: {
        chart: {
          title: 'Stock Data',
          subtitle: 'Data for Stock',
        }
      }
    }
  },
  mounted(){
  }

}
</script>

<style>

</style>