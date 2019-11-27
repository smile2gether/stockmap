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
      // append the svg object to the body of the page
      let svg = d3.select("#parallel")
        .append("svg")
        .attr("width", this.width + this.marginLeft + this.marginRight)
        .attr("height", this.height + this.marginTop + this.marginBottom)
        .append("g")
        .attr("transform",
          "translate(" + this.marginLeft + "," + this.marginTop + ")");

      // Parse the Data
      d3.csv("./data/parallel/csv", function(data) {

        // Color scale: give me a specie name, I return a color
        var color = d3.scaleOrdinal()
          .domain(["AGRO","CONSUMP","FINCIAL","INDUS","PROPCON","RESOURC","SERVICE","TECH"])
          .range(["#1f77b4", "#ff7f0e", "#2ca02c", "#d62728", "#9467bd", "#8c564b", "#e377c2", "#7f7f7f"]);

        // Here I set the list of dimension manually to control the order of axis:
        const dimensions = ["cba", "abc", "ccc", "bbb"];

        // For each dimension, I build a linear scale. I store all in a y object
        let y = {};
        const height = 500;
        for (let i in dimensions) {
          name = dimensions[i];
          y[name] = d3.scaleLinear()
            .domain( [0,200] ) // --> Same axis range for each group
            // --> different axis range for each group --> .domain( [d3.extent(data, function(d) { return +d[name]; })] )
            .range([height, 0]);
        }

        // Build the X scale -> it find the best position for each Y axis
        const width = 500;
        const x = d3.scalePoint()
          .range([0, width])
          .domain(dimensions);

        // Highlight the specie that is hovered
        let highlight = function(d) {

          let selected_specie = d.index;

          // first every group turns grey
          d3.selectAll(".line")
            .transition().duration(200)
            .style("stroke", "white")
            .style("opacity", "0.2");
          // Second the hovered specie takes its color
          d3.selectAll("." + selected_specie)
            .transition().duration(200)
            .style("stroke", color(selected_specie))
            .style("opacity", "1");
        };

        // Unhighlight
        let doNotHighlight = function(d) {
          d3.selectAll(".line")
            .transition().duration(200).delay(1000)
            .style("stroke", color(d.index))
            .style("opacity", "1")
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
          .attr("class", "line " + data.index ) // 2 class for each line: 'line' and the group name
          .attr("d",  path)
          .style("fill", "none" )
          .style("stroke", color(data.index) )
          .style("opacity", 0.5)
          .on("mouseover", highlight)
          .on("mouseleave", doNotHighlight );

        // Draw the axis:
        svg.selectAll("myAxis")
        // For each dimension of the dataset I add a 'g' element:
          .data(dimensions).enter()
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
          .style("fill", "white")
      })
    }
  },
}
</script>

<style>

</style>
