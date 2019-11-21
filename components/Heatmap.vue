<template>
  <v-container>
    <div id="dataviz"></div>
  </v-container>
</template>

<script>
import * as d3 from 'd3';
export default {
  data: () => ({
    marginTop: 30,
    marginRight: 30,
    marginBottom: 30,
    marginLeft: 30,
    myGroups: ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J"],
    myVars: ["v1", "v2", "v3", "v4", "v5", "v6", "v7", "v8", "v9", "v10"],
  }),
  computed: {
    width () {
      return 450 - this.marginLeft - this.marginRight
    },
    height () {
      return 450 - this.marginTop - this.marginBottom
    },
  },
  mounted () {
    let svg = d3.select("#dataviz")
      .append("svg")
      .attr("width", this.width + this.marginLeft + this.marginRight)
      .attr("height", this.height + this.marginTop + this.marginBottom)
      .append("g")
      .attr("transform",
        "translate(" + this.marginLeft + "," + this.marginTop + ")");
    const x = d3.scaleBand()
      .range([ 0, this.width ])
      .domain(this.myGroups)
      .padding(0.01);
    svg.append("g")
      .attr("transform", "translate(0," + this.height + ")")
      .call(d3.axisBottom(x));
    const y = d3.scaleBand()
      .range([ this.height, 0 ])
      .domain(this.myVars)
      .padding(0.01);
    svg.append("g")
      .call(d3.axisLeft(y));
    // read data
    let myColor = d3.scaleLinear()
      .range(["#fff", "#69b3a2"])
      .domain([1, 100]);
    d3.csv("https://raw.githubusercontent.com/holtzy/D3-graph-gallery/master/DATA/heatmap_data.csv", function (data) {
      console.log(data);
      svg.selectAll()
        .data(data.group+':'+data.variable)
        .enter()
        .append("rect")
        .attr("x", x(data.group))
        .attr("y", y(data.variable))
        .attr("width", x.bandwidth() )
        .attr("height", y.bandwidth() )
        .style("fill", myColor(data.value))
        // .data('A:V1')
        // .enter()
        // .append("rect")
        // .attr("x", 1)
        // .attr("y", 100)
        // .attr("width", 10 )
        // .attr("height", 10 )
        // .style("fill", 1 );
    });
  }
}
</script>
