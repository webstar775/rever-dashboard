<template>
  <svg class="bar-chart"></svg>
</template>
 
<script>
import BaseChart from 'vue-d3-basechart'
import * as d3 from 'd3'

export default BaseChart.extend({
  name: 'bar-chart',
  props: ['width', 'barHeight'],
  methods: {
    renderChart () {
      // This code is based on https://bost.ocks.org/mike/bar/2/
      var data = this.chartData
      var width = this.width
      var barHeight = this.barHeight

      var x = d3.scaleLinear()
          .domain([0, d3.max(data)])
          .range([0, width])

      var chart = d3.select(this.$el)
          .attr('width', width)
          .attr('height', barHeight * data.length)

      var d = chart.selectAll('g')
          .data(data)

      d.exit().remove()

      var g = d.enter().append('g')
          .merge(d)
          .attr('transform', function (d, i) { return 'translate(0,' + i * barHeight + ')' })
      g.selectAll('rect').remove()
      g.selectAll('text').remove()
      g.append('rect')
          .attr('width', x)
          .attr('height', barHeight - 1)
      g.append('text')
          .attr('x', function (d) { return x(d) - 3 })
          .attr('y', barHeight / 2)
          .attr('dy', '.35em')
          .text(function (d) { return d })
    }
  },
  watch: {
    width: 'renderChart',
    barHeight: 'renderChart'
  }
})
</script> 
 
<style lang="less">
.bar-chart {
  rect {
    fill: steelblue;
  }
 
  text {
    fill: white;
    font: 10px sans-serif;
    text-anchor: end;
  }
}
</style> 