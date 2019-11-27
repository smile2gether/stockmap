<template>
  <v-container>
    <!--<v-layout row wrap justify-space-around>-->
      <!--<v-flex xs5 md3>-->
        <!--<v-select-->
          <!--v-model="selectedData"-->
          <!--:items="heatMapDataList"-->
          <!--item-text="name"-->
          <!--return-object-->
          <!--label="Data"-->
          <!--outlined-->
          <!--hide-details-->
          <!--dense>-->
        <!--</v-select>-->
      <!--</v-flex>-->
      <!--<v-flex xs5 md3>-->
        <!--<v-select-->
          <!--v-model="selectedStartDate"-->
          <!--:items="startDateList"-->
          <!--label="Start Date"-->
          <!--outlined-->
          <!--hide-details-->
          <!--dense>-->
        <!--</v-select>-->
      <!--</v-flex>-->
      <!--<v-flex xs5 md3>-->
        <!--<v-select-->
          <!--v-model="selectedEndDate"-->
          <!--:items="endDateList"-->
          <!--label="End Date"-->
          <!--outlined-->
          <!--hide-details-->
          <!--dense>-->
        <!--</v-select>-->
      <!--</v-flex>-->
    <!--</v-layout>-->
    <div id="heatmap"></div>
  </v-container>
</template>

<script>
import * as d3 from 'd3';
import moment from 'moment';

export default {
  data: () => ({
    value1: [30, 60],
    marginTop: 30,
    marginRight: 30,
    marginBottom: 30,
    marginLeft: 30,
    myGroups: [],
    myVars: [],
    selectedStartDate: moment(),
    selectedEndDate: moment(),
    selectedData: {},
    heatMapDataList: [
      {
        name: 'Weekly',
        path: './data/heatmap_weekly.csv',
        dates: ["6-Jan-19","13-Jan-19","20-Jan-19","27-Jan-19","3-Feb-19","10-Feb-19","17-Feb-19","24-Feb-19","3-Mar-19","10-Mar-19","17-Mar-19","24-Mar-19","31-Mar-19","7-Apr-19","14-Apr-19","21-Apr-19","28-Apr-19","5-May-19","12-May-19","19-May-19","26-May-19","2-Jun-19","9-Jun-19","16-Jun-19","23-Jun-19","30-Jun-19","7-Jul-19","14-Jul-19","21-Jul-19","28-Jul-19","4-Aug-19","11-Aug-19","18-Aug-19","25-Aug-19","1-Sep-19","8-Sep-19","15-Sep-19","22-Sep-19","29-Sep-19"],
        sectors: ["AGRO","CONSUMP","FINCIAL","INDUS","PROPCON","RESOURC","SERVICE","TECH"]
      },
      {
        name: 'Daily',
        path: './data/heatmap.csv',
        dates: ["2-Jan-19","3-Jan-19","4-Jan-19","7-Jan-19","8-Jan-19","9-Jan-19","10-Jan-19","11-Jan-19","14-Jan-19","15-Jan-19","16-Jan-19","17-Jan-19","18-Jan-19","21-Jan-19","22-Jan-19","23-Jan-19","24-Jan-19","25-Jan-19","28-Jan-19","29-Jan-19","30-Jan-19","31-Jan-19","1-Feb-19","4-Feb-19","5-Feb-19","6-Feb-19","7-Feb-19","8-Feb-19","11-Feb-19","12-Feb-19","13-Feb-19","14-Feb-19","15-Feb-19","18-Feb-19","20-Feb-19","21-Feb-19","22-Feb-19","25-Feb-19","26-Feb-19","27-Feb-19","28-Feb-19","1-Mar-19","4-Mar-19","5-Mar-19","6-Mar-19","7-Mar-19","8-Mar-19","11-Mar-19","12-Mar-19","13-Mar-19","14-Mar-19","15-Mar-19","18-Mar-19","19-Mar-19","20-Mar-19","21-Mar-19","22-Mar-19","25-Mar-19","26-Mar-19","27-Mar-19","28-Mar-19","29-Mar-19","1-Apr-19","2-Apr-19","3-Apr-19","4-Apr-19","5-Apr-19","9-Apr-19","10-Apr-19","11-Apr-19","12-Apr-19","17-Apr-19","18-Apr-19","19-Apr-19","22-Apr-19","23-Apr-19","24-Apr-19","25-Apr-19","26-Apr-19","29-Apr-19","30-Apr-19","2-May-19","3-May-19","7-May-19","8-May-19","9-May-19","10-May-19","13-May-19","14-May-19","15-May-19","16-May-19","17-May-19","21-May-19","22-May-19","23-May-19","24-May-19","27-May-19","28-May-19","29-May-19","30-May-19","31-May-19","4-Jun-19","5-Jun-19","6-Jun-19","7-Jun-19","10-Jun-19","11-Jun-19","12-Jun-19","13-Jun-19","14-Jun-19","17-Jun-19","18-Jun-19","19-Jun-19","20-Jun-19","21-Jun-19","24-Jun-19","25-Jun-19","26-Jun-19","27-Jun-19","28-Jun-19","1-Jul-19","2-Jul-19","3-Jul-19","4-Jul-19","5-Jul-19","8-Jul-19","9-Jul-19","10-Jul-19","11-Jul-19","12-Jul-19","15-Jul-19","17-Jul-19","18-Jul-19","19-Jul-19","22-Jul-19","23-Jul-19","24-Jul-19","25-Jul-19","26-Jul-19","30-Jul-19","31-Jul-19","1-Aug-19","2-Aug-19","5-Aug-19","6-Aug-19","7-Aug-19","8-Aug-19","9-Aug-19","13-Aug-19","14-Aug-19","15-Aug-19","16-Aug-19","19-Aug-19","20-Aug-19","21-Aug-19","22-Aug-19","23-Aug-19","26-Aug-19","27-Aug-19","28-Aug-19","29-Aug-19","30-Aug-19","2-Sep-19","3-Sep-19","4-Sep-19","5-Sep-19","6-Sep-19","9-Sep-19","10-Sep-19","11-Sep-19","12-Sep-19","13-Sep-19","16-Sep-19","17-Sep-19","18-Sep-19","19-Sep-19","20-Sep-19","23-Sep-19","24-Sep-19","25-Sep-19","26-Sep-19","27-Sep-19","30-Sep-19","1-Oct-19","2-Oct-19","3-Oct-19","4-Oct-19","7-Oct-19","8-Oct-19","9-Oct-19","10-Oct-19","11-Oct-19","15-Oct-19","16-Oct-19","17-Oct-19","18-Oct-19","21-Oct-19","22-Oct-19","24-Oct-19","25-Oct-19","28-Oct-19","29-Oct-19","30-Oct-19","31-Oct-19"],
        sectors: ["AGRO","CONSUMP","FINCIAL","INDUS","PROPCON","RESOURC","SERVICE","TECH"]
      },
    ]
  }),
  computed: {
    width () {
      return document.getElementById("heatmap").offsetWidth - this.marginLeft - this.marginRight
    },
    height () {
      return window.screen.height - 400 - this.marginTop - this.marginBottom
    },
    startDate () {
      return moment(this.selectedStartDate, 'D-MMM-YY').toDate()  || moment(this.myGroups[0], 'D-MMM-YY').toDate()
    },
    endDate () {
      return moment(this.selectedEndDate, 'D-MMM-YY').toDate() || moment(this.myGroups[this.myGroups.length-1], 'D-MMM-YY').toDate()
    },
    startDateList () {
      if (this.selectedEndDate) {
        return this.myGroups.filter(date => {
          return moment(date, 'D-MMM-YY').isBefore(moment(this.selectedEndDate))
        });
      }
      return this.myGroups;
    },
    endDateList () {
      if (this.selectedStartDate) {
        return this.myGroups.filter(date => {
          return moment(date, 'D-MMM-YY').isAfter(moment(this.selectedStartDate))
        });
      }
      return this.myGroups;
    }
  },
  mounted () {
    this.selectedData = this.heatMapDataList[0];
    this.myGroups = this.selectedData['dates'];
    this.myVars = this.selectedData['sectors'];
    this.selectedStartDate = this.myGroups[0];
    this.selectedEndDate = this.myGroups[this.myGroups.length - 1];
    this.createGraph();
  },
  methods: {
    createGraph () {
      let svg = d3.select("#heatmap")
        .append("svg")
        .attr("width", this.width + this.marginLeft + this.marginRight)
        .attr("height", this.height + this.marginTop + this.marginBottom)
        .append("g")
        .attr("transform",
          "translate(" + this.marginLeft + "," + this.marginTop + ")");
      const x = d3.scaleTime()
        .domain([this.startDate, this.endDate])
        .range([0, this.width]);

      const days = moment(this.selectedEndDate, 'D-MMM-YY').diff(moment(this.selectedStartDate, 'D-MMM-YY'), 'd');
      let xAxis;
      if (days > 200) {
        xAxis = d3.axisBottom(x)
          .ticks(d3.timeMonth.every(1))
          .tickFormat(function(d, i) {
            // return "W" + (i+1) + '-' + d3.timeFormat("%d/%m/%y")(d);
            return d3.timeFormat("%d/%m/%y")(d)
          });
      } else if (days >= 15 && days < 200) {
        xAxis = d3.axisBottom(x)
          .ticks(d3.timeWeek.every(1))
          .tickFormat(function(d, i) {
            // return "W" + (i+1) + '-' + d3.timeFormat("%d/%m/%y")(d);
            return d3.timeFormat("%d/%m/%y")(d)
          });
      } else {
        xAxis = d3.axisBottom(x)
          .ticks(d3.timeDay.every(1))
          .tickFormat(function(d, i) {
            return d3.timeFormat("%d/%m/%y")(d)
          });
      }
      svg.append("g")
        .attr("transform", "translate(0," + this.height + ")")
        .call(xAxis);
      const y = d3.scaleBand()
        .range([ this.height, 0 ])
        .domain(this.myVars)
        .padding(0.01);
      svg.append("g")
        .call(d3.axisLeft(y));
      // read data
      let myColor = d3.scaleLinear()
        .range(["#fff", "#69b3a2"])
        .domain([0, 1]);
      d3.csv(this.selectedData.path, function (data) {
        data.group = moment(data.group, 'D-MMM-YY').toDate();
        svg.selectAll()
          .data(data.group+':'+data.variable)
          .enter()
          .append("rect")
          .attr("x", x(data.group))
          .attr("y", y(data.variable))
          .attr("width", 15)
          .attr("height", y.bandwidth())
          .style("fill", myColor(data.value))
      });
    },
    clearGraph () {
      d3.select("svg").remove();
    }
  },
  watch: {
    selectedData: {
      handler: function (value, oldVal) {
        if (!value) {
          this.selectedData = this.startDateList[0];
          this.myGroups = this.selectedData['dates'];
          this.myVars = this.selectedData['sectors'];
          this.selectedStartDate = this.myGroups[0];
          this.selectedEndDate = this.myGroups[this.myGroups.length-1];
        }
        if (value !== oldVal) {
          this.myGroups = value.dates;
          this.myVars = value.sectors;
          this.selectedStartDate = value.dates[0];
          this.selectedEndDate = value.dates[this.myGroups.length-1];
          this.clearGraph();
          this.createGraph();
        }
      },
      deep: true
    },
    selectedStartDate: {
      handler: function (value, oldVal) {
        if (!value) {
          this.selectedData = this.heatMapDataList[0];
        }
        if (value !== oldVal) {
          this.clearGraph();
          this.createGraph();
        }
      }
    },
    selectedEndDate: {
      handler: function (value, oldVal) {
        if (!value) {
          this.selectedEndDate = this.myGroups[this.myGroups.length - 1]
        }
        if (value !== oldVal) {
          this.clearGraph();
          this.createGraph();
        }
      }
    }
  }
}
</script>

<style>
#heatmap {
  overflow: scroll;
}
</style>
