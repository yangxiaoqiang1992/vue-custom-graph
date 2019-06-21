<template>
  <div class="graphBox" >
    <div class="canvasBox">
      <div class="dot" v-for="(item,key) in pos" :key="key" :style="{top:item.y+'px',left:item.x+'px',width:ball.width+'px',height:ball.height+'px'}"></div>
      <div v-for="(item,index) in data" class="ball" :class="'category'+item.category" :key="index"
         :style="{top:item.y+'px',left:item.x+'px'}" :data-key='item.key' :id="item.key">
         <div class="box"><span class="name">{{item.name}}</span><span class="value" v-if="item.value!=''">{{item.value}}</span></div>
      </div>
      <canvas id="graph"></canvas>
      <div class="circle circle1"></div>
      <div class="circle circle2"></div>
      <div class="circle circle3"></div>
      <div class="circle circle4"></div>
      <div class="popUp" :class="arrow">
         <div class="list">
           <ul>
             <li>闹访<span>100w</span></li>
             <li>初访<span>100w</span></li>
             <li>越级访<span>100w</span></li>
             <li>滞留访<span>100w</span></li>
             <li class="last">老户<span>100w</span></li>
           </ul>
         </div>
      </div>
    </div>
  </div>
</template>
<script>
import $ from 'jquery'
export default {
  name: 'graph',
  data () {
    return {
      /**
       category :4 对应为实体，其余为本体,根据category区分球体颜色
       */
      data: [
        {
          name: '司法统计',
          key: 'sftj',
          value: '',
          category: 0
        },
        {
          name: '年份',
          value: '',
          key: 'nf',
          category: 1
        },
        {
          name: '管辖法院',
          value: '',
          key: 'gxfy',
          category: 1
        },
        {
          name: '案件来源',
          value: '2',
          key: 'ajly',
          category: 2
        },
        {
          name: '案件类型',
          value: '23',
          key: 'ajlx',
          category: 3
        },
        {
          name: '新收案件数',
          key: 'xsajs',
          value: '45',
          category: 4
        },
        {
          name: '旧存案件数',
          key: 'jcajs',
          value: '324',
          category: 4
        },
        {
          name: '子案由',
          key: 'zay',
          value: '21',
          category: 5
        },
        {
          name: '案由',
          key: 'ay',
          value: '22',
          category: 5
        },
        {
          name: '行政行为种类',
          key: 'xzxwzl',
          value: '21',
          category: 5
        },
        {
          name: '...',
          key: 'other',
          value: '21',
          category: 5
        },
        {
          name: '未结案件数',
          key: 'wjajs',
          value: '44',
          category: 4
        },
        {
          name: '已结案件数',
          key: 'yjajs',
          value: '3',
          category: 4
        },
        {
          name: '审理程序',
          key: 'slcx',
          value: '56',
          category: 3
        },
        {
          name: '结案方式',
          key: 'jafs',
          value: '66',
          category: 2
        }
      ],
      links: [
        {
          id: '1',
          source: 'sftj',
          target: 'nf'
        },
        {
          id: '2',
          source: 'sftj',
          target: 'gxfy'
        },
        {
          id: '3',
          source: 'sftj',
          target: 'ajly'
        },
        {
          id: '4',
          source: 'sftj',
          target: 'ajlx'
        },
        {
          id: '5',
          source: 'sftj',
          target: 'xsajs'
        },
        {
          id: '6',
          source: 'sftj',
          target: 'jcajs'
        },
        {
          id: '7',
          source: 'sftj',
          target: 'zay'
        },
        {
          id: '8',
          source: 'sftj',
          target: 'ay'
        },
        {
          id: '9',
          source: 'sftj',
          target: 'xzxwzl'
        },
        {
          id: '10',
          source: 'sftj',
          target: 'other'
        },
        {
          id: '11',
          source: 'sftj',
          target: 'wjajs'
        },
        {
          id: '12',
          source: 'sftj',
          target: 'yjajs'
        },
        {
          id: '12',
          source: 'sftj',
          target: 'jafs'
        },
        {
          id: '13',
          source: 'sftj',
          target: 'slcx'
        },
        {
          id: '14',
          source: 'ajly',
          target: 'ajlx'
        },
        {
          id: '15',
          source: 'yjajs',
          target: 'ajlx'
        },
        {
          id: '16',
          source: 'wjajs',
          target: 'ajlx'
        },
        {
          id: '17',
          source: 'ay',
          target: 'ajlx'
        },
        {
          id: '18',
          source: 'ay',
          target: 'wjajs'
        },
        {
          id: '19',
          source: 'ay',
          target: 'yjajs'
        },
        {
          id: '20',
          source: 'ay',
          target: 'ajly'
        },
        {
          id: '21',
          source: 'ay',
          target: 'ajlx'
        },
        {
          id: '22',
          source: 'ay',
          target: 'other'
        },
        {
          id: '23',
          source: 'ay',
          target: 'jafs'
        },
        {
          id: '24',
          source: 'ay',
          target: 'zay'
        },
        {
          id: '25',
          source: 'slcx',
          target: 'zay'
        },
        {
          id: '26',
          source: 'slcx',
          target: 'jafs'
        },
        {
          id: '27',
          source: 'slcx',
          target: 'ajly'
        },
        {
          id: '28',
          source: 'slcx',
          target: 'yjajs'
        },
        {
          id: '29',
          source: 'slcx',
          target: 'wjajs'
        },
        {
          id: '30',
          source: 'xsajs',
          target: 'jcajs'
        },
        {
          id: '31',
          source: 'slcx',
          target: 'xzxwzl'
        },
        {
          id: '32',
          source: 'ajly',
          target: 'xzxwzl'
        },
        {
          id: '33',
          source: 'ajly',
          target: 'xsajs'
        },
        {
          id: '34',
          source: 'ajly',
          target: 'jafs'
        },
        {
          id: '35',
          source: 'ay',
          target: 'xsajs'
        },
        {
          id: '36',
          source: 'zay',
          target: 'ajlx'
        },
        {
          id: '37',
          source: 'zay',
          target: 'slcx'
        },
        {
          id: '38',
          source: 'zay',
          target: 'jcajs'
        },
        {
          id: '39',
          source: 'ajlx',
          target: 'jcajs'
        },
        {
          id: '40',
          source: 'ajlx',
          target: 'xzxwzl'
        }
      ],
      colors: [{start: '#ffad94', end: '#f17b6b'}, {start: '#d56ac0', end: '#fb9ce3'}, {start: '#73baf6', end: '#025dcd'}, {start: '#35c6e0', end: '#0190cc'}, {start: '#bc8df0', end: '#7b3fbe'},
        {start: '#18daaa', end: '#009b92'}],
      graphStyle: '',
      pos: {},
      ellipse: {
        a: 558,
        b: 200
      },
      center: {
        x: 640,
        y: 320
      },
      ball: {
        width: 50,
        height: 50
      },
      alpha: '',
      beta: '',
      posObj: '',
      graphEcharts: null,
      option: null,
      step: 0,
      step1: 0,
      timer: null,
      timer1: null,
      arrow: null
    }
  },
  methods: {
    route (context, x, y, a, b) {
      var _this = this
      var step = (a > b) ? 1 / a : 1 / b
      context.save()
      context.translate(_this.center.x, _this.center.y)
      context.beginPath()
      context.setLineDash([14, 5])
      context.lineWidth = 3
      context.strokeStyle = '#cfe7f6'
      context.moveTo(x + a, y) // 从椭圆的左端点开始绘制
      for (var i = 0; i < 2 * Math.PI; i += step) {
        // 参数方程为x = a * cos(i), y = b * sin(i)，
        // 参数为i，表示度数（弧度）
        context.lineTo(x + a * Math.cos(i), y + b * Math.sin(i))
      }
      context.closePath()
      context.stroke()
      context.restore()
    },
    /**
      @param sub :积分思想，设置分段数量，用于将椭圆角度细分
    */
    splitAngle (sub) {
      this.alpha = parseFloat(360 / (this.data.length - 3))
      this.beta = 360 / sub
    },
    getNextPos (startAngel, endAngel, obj, size, speed) {
      var _this = this
      var curAngel = startAngel
      _this.posObj = {
        x: '',
        y: ''
      }
      // 执行一段弧度动画
      if (curAngel < endAngel) {
        curAngel = startAngel + _this.step1
      } else {
        curAngel = startAngel
      }
      _this.posObj.x = _this.center.x + (_this.ellipse.a + 80) * Math.cos((curAngel) * Math.PI / 180) - (size / 2)
      _this.posObj.y = _this.center.y + (_this.ellipse.b + 70) * Math.sin((curAngel) * Math.PI / 180) - (size / 2)
      obj.animate({
        top: _this.posObj.y + 'px',
        left: _this.posObj.x + 'px'
      }, speed)
      // 时间必须足够小，满足视觉暂留原理
    //  }
    },
    createLinks (ctx, width, height, key) {
      var _this = this
      ctx.clearRect(0, 0, width, height)
      this.route(ctx, 0, 0, _this.ellipse.a + 80, _this.ellipse.b + 70)
      var linkedArr = []
      this.links.forEach(function (item, index) {
        var x = _this.pos[item.source].x
        var y = _this.pos[item.source].y
        var x1 = _this.pos[item.target].x
        var y1 = _this.pos[item.target].y
        var category = _this.pos[item.source].category
        var color = ''
        if (key && item.source == key) {
          linkedArr.push(item)
        }
        if (category == '0') {
          if (key && item.source != key) {
            color = 'rgba(241,145,132,.2)'
          } else {
            color = '#f19184'
          }
        } else if (category == '1') {
          if (key && item.source != key) {
            color = 'rgba(213,106,192,.2)'
          } else {
            color = '#d56ac0'
          }
        } else if (category == '2') {
          if (key && item.source != key) {
            color = 'rgba(2,93,205,.2)'
          } else {
            color = '#025dcd'
          }
        } else if (category == '3') {
          if (key && item.source != key) {
            color = 'rgba(1,144,204,.2)'
          } else {
            color = '#0190cc'
          }
        } else if (category == '4') {
          if (key && item.source != key) {
            color = 'rgba(123,63,190,.2)'
          } else {
            color = '#7b3fbe'
          }
        } else if (category == '5') {
          if (key && item.source != key) {
            color = 'rgba(0,155,146,.2)'
          } else {
            color = '#009b92'
          }
        } else {
          color = '#a8cdf0'
        }
        ctx.save()
        ctx.beginPath()
        ctx.strokeStyle = color
        ctx.moveTo(x, y)
        ctx.lineTo(x1, y1)
        ctx.stroke()
        ctx.restore()
      })
      if (key) {
        $('.ball').css('opacity', 0.1)
        linkedArr.forEach(function (item, index) {
          $('#' + item.target).css('opacity', 1)
          $('#' + key).css('opacity', 1)
        })
      } else {
        $('.ball').css('opacity', 1)
      }
    },
    computedStyleandPos () {
      var _this = this
      this.step += this.beta
      _this.pos = []
      this.data.forEach(function (item, index) {
        var key = item.key
        var temp = {
          name: '',
          x: '',
          y: '',
          category: ''
        }
        if (item.name == '司法统计') {
          item.x = _this.center.x - 80
          item.y = _this.center.y - 80 - (_this.ellipse.b)
        } else {
          if (item.name == '年份' || item.name == '管辖法院') {
            if (item.name == '年份') {
              item.x = _this.center.x + 60
            } else {
              item.x = _this.center.x - 160
            }
            item.y = _this.center.y - 200 - (_this.ellipse.b)
          } else {
            var startAngel = -90 - _this.alpha * (index - 3)
            var curAngel = startAngel
            var endAngel = 270 - _this.alpha * (index - 3)
            if (curAngel < endAngel) {
              curAngel += _this.step
            } else {
              curAngel = startAngel
            }
            item.x = _this.center.x + (_this.ellipse.a) * Math.cos(curAngel * Math.PI / 180) - 63
            item.y = _this.center.y + (_this.ellipse.b) * Math.sin(curAngel * Math.PI / 180) - 63
          }
        }
        temp.name = item.name
        temp.category = item.category
        if (item.name == '司法统计') {
          temp.x = item.x + 80
          temp.y = item.y + 80
        } else {
          temp.x = item.x + 63
          temp.y = item.y + 63
        }
        _this.$set(_this.pos, key, temp)
      })
    }
  },
  mounted () {
    var _this = this
    this.splitAngle(1000)
    var canvas = document.getElementById('graph')
    canvas.width = 1280
    canvas.height = 640
    var ctx = canvas.getContext('2d')
    this.step = this.beta
    this.timer = setInterval(function () {
      _this.computedStyleandPos()
      _this.createLinks(ctx, canvas.width, canvas.height, null)
    }, 60)
    this.timer1 = setInterval(function () {
      _this.getNextPos(0, 360, $('.circle1'), 15, 20)
      _this.getNextPos(180, 540, $('.circle3'), 8, 20)
      _this.getNextPos(-5, 355, $('.circle2'), 8, 20)
      _this.getNextPos(178, 537, $('.circle4'), 4, 20)
      _this.step1 += _this.beta
    }, 30)

    $('.ball').hover(function () {
      clearInterval(_this.timer)
      clearInterval(_this.timer1)
      var key = $(this).data('key')
      _this.createLinks(ctx, canvas.width, canvas.height, key)
      $('#' + key).css('opacity', 1)
      var xy = _this.pos[key]
      var boxW = $('.popUp').width()
      var top = xy.y - parseInt($('.popUp').height()) / 2
      var left = xy.x + parseInt($(this).width()) * 0.6
      if (left + boxW > $('.canvasBox').width()) {
        left = xy.x - boxW - parseInt($(this).width()) * 0.7
        _this.arrow = 'right'
      } else {
        _this.arrow = 'left'
      }
      $('.popUp').css({'top': top + 'px', 'left': left + 'px'}).animate({'opacity': 1}, 500).show()
    }, function () {
      $('.popUp').hide()
      _this.timer = setInterval(function () {
        _this.computedStyleandPos()
        _this.createLinks(ctx, canvas.width, canvas.height, null)
      }, 60)
      _this.timer1 = setInterval(function () {
        _this.getNextPos(0, 360, $('.circle1'), 15, 20)
        _this.getNextPos(180, 540, $('.circle3'), 8, 20)
        _this.getNextPos(-5, 355, $('.circle2'), 8, 20)
        _this.getNextPos(177, 538, $('.circle4'), 4, 20)
        _this.step1 += _this.beta
      }, 30)
    })
  }
}
</script>

<style scoped>
.graphBox{
    height:calc(100% - 68px);
    background:url(../../assets/images/bg.png) no-repeat center;
    background-size:100% 100%;
    position:relative;
    overflow:auto;
}
.dot{
  border-radius:100%;
  background:transparent;
  position:absolute;
  z-index:4;
}
.canvasBox{
  width:1280px;
  height:640px;
  margin:auto;
  position:relative;
  z-index:2;
  margin-top:120px;
  background:url(../../assets/images/circle1.png) no-repeat 5% 50%;
  background-size:101% 87%;
}
#echarts{
  width:1280px;
  height:640px;
  position:absolute;
  top:0;
  left:0;
  z-index:1;
}
#graph{
  width:1280px;
  height:640px;
  position:relative;
  z-index:2;
}
.graphBg{
  /* background:url(../../assets/images/circle1.png) no-repeat center; */
  background-size:100% 100%;
  position:absolute;
  left:0;
  right:0;
  margin:auto;
  z-index:1;
}
.ball{
  width:126px;
  height:126px;
  display:table;
  position:absolute;
  z-index:3;
}
.ball:hover{
  transform:scale(1.1);
  -webkit-transform:scale(1.1);
  -moz-transform:scale(1.1);
  transition:transform 0.2s;
}
.ball.category0{
 width:161px;
 height:161px;
 background:url(../../assets/images/symbol0.png) no-repeat center;
 z-index:10;
}
.ball.category1{
 background:url(../../assets/images/symbol1.png) no-repeat center;
}
.ball.category2{
 background:url(../../assets/images/symbol2.png) no-repeat center;
}
.ball.category3{
 background:url(../../assets/images/symbol3.png) no-repeat center;
}
.ball.category4{
 background:url(../../assets/images/symbol4.png) no-repeat center;
}
.ball.category5{
 background:url(../../assets/images/symbol5.png) no-repeat center;
}
.box{
  display:table-cell;
  vertical-align:middle;
}
.box span{
  display:inline-block;
  width:100%;
}
.ball.category0 span.name{
  font-size:24px;
  text-shadow:0 6px 7px rgba(223,81,60,.5);
  padding-top:25px;
}
.box span.name{
  color:#feffff;
  font-size:18px;
  font-weight:bold;
  padding:0 20%;
  text-align:center;
}
.box span.value{
  color:#fff;
  font-size:22px;
  font-weight:bold;
  padding-top:8px;
}
.circle{
  position:absolute;
  z-index:3;
}
.circle1{
 width:15px;
 height:15px;
 border-radius:15px;
 background:#208af3;
}
.circle2{
 width:8px;
 height:8px;
 border-radius:8px;
 background:#8cc4f8;
}
.circle3{
 width:8px;
 height:8px;
 border-radius:8px;
 background:#208af3;
}
.circle4{
 width:4px;
 height:4px;
 border-radius:4px;
 background:#208af3;
}
.popUp{
  position:absolute;
  background:#dcf5fc;
  padding:6px;
  border:2px solid #bce2f7;
  box-shadow:0 4px 7px  rgba(26,92,166,.35);
  border-radius:8px;
  -webkit-border-radius:8px;
  -moz-border-radius:8px;
  position: absolute;
  z-index:100;
  width:264px;
  display:none;
}
.popUp.left:before{
  content:'';
  width:10px;
  height:10px;
  position:absolute;
  top:0;
  bottom:0;
  border-left:2px solid #bce2f7;
  border-top:2px solid #bce2f7;
  background:#e0f4fc;
  transform:rotate(-45deg);
  margin:auto;
  left:-8px;
  z-index:1;
}
.popUp.right:after{
  content:'';
  width:10px;
  height:10px;
  position:absolute;
  top:0;
  bottom:0;
  border-right:2px solid #bce2f7;
  border-bottom:2px solid #bce2f7;
  background:#e0f4fc;
  transform:rotate(-45deg);
  margin:auto;
  right:-8px;
  z-index:1;
}
.list{
  background:#f3fbfe;
  border-radius:4px;
  -webkit-border-radius:4px;
  -moz-border-radius:4px;
  width:248px;
  height:168px;
}
.list ul{
  padding:5px 7px;
}
.list ul li{
  height:32px;
  line-height:32px;
  border-bottom:1px solid #d9effb;
  text-align:left;
  color:#025394;
  font-weight:bold;
}
.list ul li span{
  float:right;
  font-weight:normal;
  color:#399cd4;
}
.list ul li.last{
 border:none;
}
</style>
