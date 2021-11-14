<template>
  <div ref="mapbox" style="min-height: 100vh;min-width: 100vw;">

  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import jsonp from 'jsonp'

const option = {
  // 标题
  title: {
    text: '全国疫情地图一览',
    left: 'center',
    top: '14px',
    textStyle: {
      fontSize: '28',
      fontFamily: 'serif'
    }
  },
  series: [
    {
      // 数据
      data: [],
      // 图表类型
      type: 'map',
      // 地区
      map: 'china',
      // 标签-地区名
      label: {
        show: true,
        // color: 'black',
        fontSize: 12
      },
      // 普通样式
      itemStyle: {
        areaColor: 'pink',
        borderColor: '#776a6a',
      },
      // 高亮样式
      emphasis: {
        label: {
          color: 'black',
          fontSize: 12,
          formatter: function(param) {
            return param.data.value
          }
        },
        itemStyle: {
          areaColor: '#ccc'
        }
      },
      zoom: 1
    }
  ],
  // 分层次显示地图范围，即类目项
  visualMap: [
    {
      zoom: 1,
      // 条状
      type: 'piecewise',
      splitNumber: 5,
      pieces: [
        { min: 10000 },
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { min: 1, max: 9 }
      ],
      align: 'left',
      inRange: {
        symbol: 'rect',
        color: [ '#ffc9c9', '#9c0505' ]
      },
      itemWidth: 20,
      itemHeight: 10
    }
  ]
}

export default {
  name: 'EpidemicMap',
  mounted() {
    this.getData()
    this.mychart = echarts.init(this.$refs.mapbox)
    this.mychart.setOption(option)
  },
  methods: {
    getData() {
      jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427&callback=__jp0', {}, (err, data) => {
        if (!err) {
          console.log("疫情数据:", data)
          let list = data.data.list.map(item => ({
            name: item.name,
            value: item.value
          }))
          console.log("格式化后数据为:");
          option.series[0].data = list
          this.mychart.setOption(option)
        }
      })
    }
  }
}
</script>

<style>

</style>