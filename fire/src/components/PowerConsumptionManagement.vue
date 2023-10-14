<template>
  <div class="power-consumption">
    <title-banner title="能耗管理"/>
    <div class="time-range-filters">
      <div 
        class="filter"
        @click="timeRange = 'today'"
        :style="timeRange == 'today' ? activeTimeRangeStyle : inactiveTimeRangeStyle"
      >今日用电</div>
      <div 
        class="filter"
        @click="timeRange = 'week'"
        :style="timeRange == 'week' ? activeTimeRangeStyle : inactiveTimeRangeStyle"
      >本周用电</div>
    </div>

    <div class="power-board">
      <div class="board usage">
        <div class="title">当前用电总量</div>
        <div class="prim-data">
          <span>{{powerConsumedToday}}</span>
          KW·h
        </div>

        <div class="second-data">
          昨日用电  {{powerConsumedYesterday}} KW·h
        </div>
        <div class="comparison">
          对比昨日 +200%
        </div>
      </div>

      <div class="board peak">
        <div class="title">当前用电高峰</div>
        <div class="prim-data">
          <span>{{powerPeak}}</span>
        </div>

        <div class="second-data">
          用电量 102KW·h
        </div>
        <div class="comparison">
          回比去年今日 -12%
        </div>
      </div>
    </div>
    <div class="chart" ref="chart"></div>
  </div>
</template>

<script>
import TitleBanner from './TitleBanner.vue';

function draw(data, el) {
  var echarts = require('echarts');
  let mychart = echarts.init(el, null, {
    width: el.offsetWidth,
    height: el.offsetHeight
  });
  mychart.setOption({
    grid: {
      top: 10,
      bottom: 30
    },
    xAxis: {
      type: 'value',
      interval: 14400,
      min: 0,
      max: 86400,
      splitLine: {
        show: false
      },
      axisLabel: {
        formatter: function(seconds) {
          const hour = Math.floor(seconds / 3600);
          return `${hour}:00`
        }
      }
    },
    yAxis: {
      type: 'value',
      splitLine: {
        show: true,
        lineStyle: {
          color: ["gray"]
        }
      }
    },
    series: [
      {
        data,
        type: 'line',
        smooth: true,
        showSymbol: false
      }
    ]
  });
}

function getData(size) {
  const atom_seconds = Math.floor(86400 / size);
  return Array.from({length: size}, (_, index) => [
    index * atom_seconds,
    Math.floor(Math.random() * 1000)
  ])
}

export default {
  name: "PowerConsumption",
  setup: function() {
    return {
      activeTimeRangeStyle: {
        borderColor: "rgb(71,105,248)",
        pointerEvents: "none"
      },
      inactiveTimeRangeStyle: {
        borderColor: "white",
        cursor: "pointer"
      },
      powerConsumedToday: 306,
      powerConsumedYesterday: 102,
      powerPeak: "02:00-03:59"
    }
  },
  data: function() {
    return {
      timeRange: "today"
    }
  },
  mounted: function() {
    draw(getData(100), this.$refs.chart);
  },
  components: {
    TitleBanner
  }
}
</script>

<style scoped>
.power-consumption {
  display: flex;
  flex-direction: column;
}
.time-range-filters {
  width: 100%;
  height: 30px;
  display: flex;
  margin-top: 20px;
  flex: none;
}
.time-range-filters .filter {
  flex: none;
  width: 25%;
  border-width: 1px;
  border-style: solid;
  font-size: 70%;
  text-align: center;
  top: 0; bottom: 0;
  margin-right: 10px;
}

.power-board {
  display: flex;
  width: 100%;
  height: 30%;
  flex: none;
}

.power-board .board {
  flex: 1;
  margin: 10px;
  padding-left: 10px;
}

.power-board .board .title {
  font-size: 80%;
  height: 30px;
  color: gray;
}

.power-board .board .prim-data {
  height: 30%;
  font-size: 20px;
  color: gray;
}
.power-board .board .prim-data span {
  font-size: 30px;
  color: white;
  margin-right: 10px;
} 
.power-board .board .second-data {
  height: 20%;
  color: gray;
  font-size: 80%;
  margin-bottom: 5px;
}
.power-board .board .comparison {
  height: 20%;
  font-size: 80%;
}
.power-board .usage .comparison {
  color: red;
}
.power-board .peak .comparison {
  color: green;
}

.chart {
  flex: auto;
}
</style>
