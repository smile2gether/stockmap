<template>
  <v-container>
    <div id="parallel"></div>
  </v-container>
</template>

<script>
import * as d3 from 'd3';

export default {
  data: () => ({
    marginTop: 30,
    marginRight: 50,
    marginBottom: 10,
    marginLeft: 50,
    sectors: ["AGRO","CONSUMP","FINCIAL","INDUS","PROPCON","RESOURC","SERVICE","TECH"],
    colors: [ "#1f77b4", "#ff7f0e", "#2ca02c", "#d62728", "#9467bd", "#8c564b", "#e377c2", "#7f7f7f"],
    dimensions: ["cba", "abc", "ccc", "bbb"],
  }),
  computed: {
    width () {
      return document.getElementById("parallel").offsetWidth - this.marginLeft - this.marginRight
    },
    height () {
      return document.getElementById("heatmap").offsetHeight - 10 - this.marginTop - this.marginBottom;
    }
  },
  mounted () {
    this.createGraph();
  },
  methods: {
    createGraph () {
      // Color scale: give me a specie name, I return a color
      const color = d3.scaleOrdinal()
        .domain(this.sectors)
        .range(this.colors);

      // Here I set the list of dimension manually to control the order of axis:
      const dimensions = this.dimensions;

      // For each dimension, I build a linear scale. I store all in a y object
      let y = [];
      for (let i in dimensions) {
        name = dimensions[i];
        y[name] = d3.scaleLinear()
          .domain( [0,200] ) // --> Same axis range for each group
          .range([this.height, 0])
      }

      // Build the X scale -> it find the best position for each Y axis
      const x = d3.scalePoint()
        .range([0, this.width])
        .domain(dimensions);

      // append the svg object to the body of the page
      let svg = d3.select("#parallel")
        .append("svg")
        .attr("width", this.width + this.marginLeft + this.marginRight)
        .attr("height", this.height + this.marginTop + this.marginBottom)
        .append("g")
        .attr("transform",
          "translate(" + this.marginLeft + "," + this.marginTop + ")");

      // Draw the axis:
      svg.selectAll("myAxis")
      // For each dimension of the dataset I add a 'g' element:
        .data(this.dimensions).enter()
        .append("g")
        .attr("class", "axis")
        // I translate this element to its right position on the x axis
        .attr("transform", function(d) { return "translate(" + x(d) + ")"; })
        // And I build the axis with the call function
        .each(function(d) { d3.select(this).call(d3.axisLeft().ticks(5).scale(y[d])); })
        // Add axis title
        .append("text")
        .style("text-anchor", "middle")
        .attr("y", -9)
        .text(function(d) { return d; })
        .style("fill", "white");

      // Parse the Data
      d3.csv("./data/parallel.csv").then(function(data) {
        // Highlight the specie that is hovered
        let highlight = function (d) {
          console.log('HH', d);
          const selected_specie = d.index;
          // first every group turns grey
          d3.selectAll(".line")
            .transition().duration(200)
            .style("stroke", "lightgrey")
            .style("opacity", "0.2");
          // Second the hovered specie takes its color
          d3.selectAll("." + selected_specie)
            .transition().duration(200)
            .style("stroke", color(selected_specie))
            .style("opacity", "1");
        };

        // Unhighlight
        let doNotHighlight = function (d) {
          d3.selectAll(".line")
            .transition().duration(200).delay(1000)
            .style("stroke", color(d.index))
            .style("opacity", "1");
        };

        // The path function take a row of the csv as input, and return x and y coordinates of the line to draw for this raw.
        function path(d) {
          return d3.line()(dimensions.map(function(p) { return [x(p), y[p](d[p])]; }));
        }

        // Draw the lines
        svg
          .selectAll("myPath")
          .data(data)
          .enter()
          .append("path")
          .attr("class", (d) => "line " + d.index ) // 2 class for each line: 'line' and the group name
          .attr("d",  path)
          .style("fill", "none" )
          .style("stroke", (d) => color(d.index) )
          .style("opacity", 0.5)
          .on("mouseover", highlight)
          .on("mouseleave", doNotHighlight );
      })
    }
  },
}
</script>

<style>

</style>
