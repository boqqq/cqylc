<template>
    <div id="index">
        <el-row :gutter="2">
            <!--左侧部分 -->
            <el-col :span="8">
                <div class="leftBox">
                    <div class="tltleText"><h4>交通大数据辅助决策</h4></div>
                    <div class="bar">
                        <div id="chartsBar_left"></div>
                        <button class="yardStop" @click="barYardStop1(1)" :class="{ active: isActive1==1 }">停车场</button>
                        <button class="yardStop_no" @click="barYardStop1(2)" :class="{ active: isActive1==2}">非停车场</button>
                    </div>
                    <div class="Line">
                        <div id="chartsLine" ></div>
                        <button class="yardStop" @click="barYardStop2(1)" :class="{ active: isActive2==1 }">停车场</button>
                        <button class="yardStop_no" @click="barYardStop2(2)" :class="{ active: isActive2==2}">非停车场</button>
                    </div>
                    <div class="Line_Bg">
                        <div id="chartsLine_Bg"></div>
                        <button class="yardStop" @click="barYardStop3(1)" :class="{ active: isActive3==1 }">停车场</button>
                        <button class="yardStop_no" @click="barYardStop3(2)" :class="{ active: isActive3==2}">非停车场</button>
                    </div>
                </div>
            </el-col>
            <!--地图部分 -->
            <el-col :span="6">
                 <div id="allmap" ref="allmap"></div>
            </el-col>
            <!--右侧部分 -->
            <el-col :span="10">
                <div class="rightBox">
                    <div class="carPinCharts">
                        <div id="chartsCar"></div>
                        <div id="chartsPie"></div>
                    </div>
                    <div class="Line_BgCharts">
                        <div id="chartsLine_color"></div>
                        <div id="chartsLine_Bg_right"></div>
                    </div>
                    <div id="chartsBar_right"></div>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import BaiduMap from 'BaiduMap';
import axios from 'axios';
var echarts = require('echarts');
export default {
    name:"index",
    components: {
        BaiduMap
    },
    data(){
        return{
            center: {lng: 0, lat: 0},
            zoom: 3,
            isActive1:1,
            isActive2:1,
            isActive3:1,
        }
    },
    methods: {
        map(){
            let map =new BMap.Map(this.$refs.allmap); // 创建Map实例
            map.centerAndZoom(new BMap.Point(116.404, 39.915), 11);// 初始化地图,设置中心点坐标和地图级别
            map.addControl(new BMap.MapTypeControl({//添加地图类型控件
                mapTypes:[
                BMAP_NORMAL_MAP,
                BMAP_HYBRID_MAP
                ]}));
            map.setCurrentCity("北京");// 设置地图显示的城市 此项是必须设置的
            map.enableScrollWheelZoom(true); //开启鼠标滚轮缩放
        },
        barYardStop1(key){//停车场按钮
            if(key == 1){
                this.isActive1 = 1;
            }
            else if(key == 2){
                this.isActive1 = 2;
            }
        },
        barYardStop2(key){//停车场按钮
            if(key == 1){
                this.isActive2 = 1;
            }
            else if(key == 2){
                this.isActive2 = 2;
            }
        },
        barYardStop3(key){//停车场按钮
            if(key == 1){
                this.isActive3 = 1;
            }
            else if(key == 2){
                this.isActive3 = 2;
            }
        },
        chartsBar(){//左侧第一个柱状图
            var chartsBar = echarts.init(document.getElementById('chartsBar_left'));
            chartsBar.setOption({
                color: ['#2AB7FF'],
                title: {
                    text: '接驳点人群聚集',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                tooltip: {},
                xAxis: {
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            // fontStyle:"oblique",
                            fontSize:12, 
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    },
                    data: ["9:00","9:05","9:10","9:15","9:20","9:25","9:30","9:35","9:40","9:45","9:50","9:55","10:00"]
                },
                yAxis: {
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    }
                },
                series: [{
                    name: '接驳点人群聚集',
                    type: 'bar',
                    barGap:'-100%',
                    barCategoryGap:'50%',
                    // barWidth: '40%',
                    data: [100, 200, 400, 200, 800, 600,80, 800, 500, 600, 300, 700,200]
                }]
            });
            window.onresize = chartsBar.resize;
        },
        chartsLine(){ //左侧第二个折线图
            var chartsLine = echarts.init(document.getElementById('chartsLine'));
            chartsLine.setOption({
                color: ['#2AB7FF'],
                title: {
                    text: '不同接驳点人群聚集',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                tooltip: {
                    trigger: 'axis'
                },
                legend: {
                    // color:["#ffffff","#000"],
                    icon: "rect",
                    itemWidth:8,//图例的宽度
                    itemHeight:8,//图例的高度
                    textStyle:{//图例文字的样式
                        color:'#b3e3fc',
                        fontSize:12
                    },
                    left:'left',
                    top:'15%',
                    data:['邮件营销','联盟广告']
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '3%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            // fontStyle:"oblique",
                            fontSize:12, 
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    },
                    data: ["9:00","9:05","9:10","9:15","9:20","9:25","9:30","9:35","9:40","9:45","9:50","9:55","10:00"]
                },
                yAxis: {
                    type: 'value',
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    }
                },
                series: [
                    {
                        name:'邮件营销',
                        type:'line',
                        stack: '总量',
                        data:[120, 132, 101, 134, 90, 230, 210,120, 132, 101, 134, 90, 230, 210]
                    },
                    {
                        name:'联盟广告',
                        type:'line',
                        stack: '总量',
                        data:[220, 182, 191, 234, 290, 330, 310,120, 132, 101, 134, 90, 230, 210]
                    },
                ]
            })
            window.onresize = chartsLine.resize;
        },
        chartsLine_Bg(){ //左侧第三个折线图
            var chartsLine_Bg = echarts.init(document.getElementById('chartsLine_Bg'));
            chartsLine_Bg.setOption({
                color: ['#2AB7FF'],
                title: {
                    text: '未来20分钟接驳等待人数预估',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                tooltip : {
                    trigger: 'axis',
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                legend: {
                    itemWidth:10,//图例的宽度
                    itemHeight:10,//图例的高度
                    textStyle:{//图例文字的样式
                        color:'#b3e3fc',
                        fontSize:12
                    },
                    left:'left',
                    top:'15%',
                    data:['当前接待人数','未来20分钟接待人数']
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '3%',
                    containLabel: true
                },
                xAxis : [
                    {
                        type : 'category',
                        boundaryGap : false,
                        axisLabel:{
                        textStyle:{
                            color:"#849098",
                            // fontStyle:"oblique",
                            fontSize:12, 
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    },
                    data : ['接驳点1','接驳点2','接驳点3','接驳点4','接驳点5','接驳点6','接驳点7','接驳点8']
                    }
                ],
                yAxis : [
                    {
                        type : 'value',
                        axisLabel:{
                            textStyle:{
                                color:"#849098",
                                
                            }
                        },
                        axisLine:{
                            lineStyle:{
                                color:'#fff',
                                width:1,//这里是为了突出显示加上的
                            }
                        }
                    }
                ],
                series : [
                    {
                        name:'当前接待人数',
                        type:'line',
                        stack: '总量',
                        areaStyle: {},
                        data:[120, 132, 101, 134, 90, 230, 210, 101]
                    },
                    {
                        name:'未来20分钟接待人数',
                        type:'line',
                        stack: '总量',
                        areaStyle: {},
                        data:[220, 182, 191, 234, 290, 330, 310, 101]
                    },
                ]
            })
            window.onresize = chartsLine_Bg.resize;
        },
        chartsCar(){//右侧接驳点运力分配
            var chartsCar = echarts.init(document.getElementById('chartsCar'));
            var img1 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img2 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img3 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img4 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img5 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img6 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img7 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var img8 = {
                a: "../../static/img/car.png",
                b: "../../static/img/carImg.png"
            }
            var value = 1; // >0
            chartsCar.setOption({
                title: {
                    text: "接驳点运力分配",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    },
                    left: '3%',
                    top:"3%",
                },
                grid: {
                    left: "3%",
                    top: "7%",
                    bottom: "16%",
                    containLabel: true
                },
                tooltip: {
                    trigger: "item",
                },
                xAxis: {
                    splitLine: {
                        show: false
                    },
                    axisLine: {
                        show: false
                    },
                    axisLabel: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    }
                },
                yAxis: [{
                        type: "category",
                        inverse: false,
                        data: ["接驳点1", "接驳点2", "接驳点3", "接驳点4", "接驳点5","接驳点6","接驳点7","接驳点8"],
                        axisLine: {
                            show: false
                        },
                        axisTick: {
                            show: false
                        },
                        splitLine: {
                            show: false,
                            lineStyle: {
                                type: "dashed",
                                color: "#3e86dd"
                            }
                        },
                        axisLabel: {
                            margin: 35,
                            textStyle: {
                                color: "#fff",
                                fontSize: 12,
                            }
                        }
                    }
                ],
                series: [
                    {
                        tooltip: {
                            show: false
                        },
                        z: 4,
                        barGap:'90%',/*多个并排柱子设置柱子之间的间距*/
  	                    barCategoryGap:'80%',/*多个并排柱子设置柱子之间的间距*/
                        type: "pictorialBar",
                        symbolSize: ['30', '30'],
                        symbolRepeat: "fixed",
                        data: [{
                                value: value,
                                symbol: 'image://' + img1.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img2.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img3.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img4.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img5.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img3.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img4.b,
                            },
                            {
                                value: value,
                                symbol: 'image://' + img5.b,
                            },
                        ]
                    },
                    {
                        z: 6,
                        type: "pictorialBar",
                        barGap:'90%',/*多个并排柱子设置柱子之间的间距*/
  	                    barCategoryGap:'80%',/*多个并排柱子设置柱子之间的间距*/
                        symbolSize: ['30', '30'],
                        animation: true,
                        symbolRepeat: "fixed",
                        symbolClip: true,
                        symbolPosition: "start",
                        symbolOffset: [0, 0],
                        data: [{
                                value: 60,
                                symbol: 'image://' + img1.a,
                            },
                            {
                                value: 85,
                                symbol: 'image://' + img2.a,
                            },
                            {
                                value: 105,
                                symbol: 'image://' + img3.a,
                            },
                            {
                                value: 130,
                                symbol: 'image://' + img4.a,
                            },
                            {
                                value: 185,
                                symbol: 'image://' + img5.a,
                            },
                            {
                                value: 105,
                                symbol: 'image://' + img6.a,
                            },
                            {
                                value: 130,
                                symbol: 'image://' + img7.a,
                            },
                            {
                                value: 185,
                                symbol: 'image://' + img8.a,
                            }
                        ],
                        label: {
                            normal: {
                                show: true,
                                textStyle: {
                                    color: '#18fcff',
                                    fontSize: 14,

                                },
                                position: "right",
                                offset: [35, 0]
                            }
                        },
                    },
                    {
                        type: "bar",
                        
                    },
                ]
            })
            window.onresize = chartsCar.resize;
        },
        chartsPie(){//右侧接驳点运力缺口
            var chartsPie = echarts.init(document.getElementById('chartsPie'));
            chartsPie.setOption({
                title: {
                    text: '接驳点运力缺口',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                tooltip: {
                    trigger: 'item',
                    formatter: "{a} <br/>{b}: {c} ({d}%)"
                },
                series: [
                    {
                        name:'接驳点运力',
                        type:'pie',
                        radius: ['50%', '70%'],
                        avoidLabelOverlap: false,
                        label: {
                            normal: {
                                show: false,
                                position: 'center'
                            },
                            emphasis: {
                                show: true,
                                textStyle: {
                                    fontSize: '30',
                                    fontWeight: 'bold'
                                }
                            }
                        },
                        labelLine: {
                            normal: {
                                show: false
                            }
                        },
                        data:[
                            {value:335, name:'接驳点1'},
                            {value:310, name:'接驳点2'},
                            {value:234, name:'接驳点3'},
                            {value:135, name:'接驳点4'},
                            {value:1548, name:'接驳点5'},
                            {value:1548, name:'接驳点6'},
                            {value:1548, name:'接驳点7'},
                            {value:1548, name:'接驳点8'},
                        ]
                    }
                ]
            })
            window.onresize = chartsPie.resize;
        },
        chartsLine_color(){//右侧运力变化趋势
            var chartsLine_color = echarts.init(document.getElementById('chartsLine_color'));
            chartsLine_color.setOption({
                color: ['#2AB7FF'],
                title: {
                    text: '运力变化趋势',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    },
                },
                tooltip: {
                    trigger: 'axis'
                },
                legend: {
                    // color:["#ffffff","#000"],
                    icon: "rect",
                    itemWidth:8,//图例的宽度
                    itemHeight:8,//图例的高度
                    textStyle:{//图例文字的样式
                        color:'#b3e3fc',
                        fontSize:12
                    },
                    left: '12%',
                    top:'15%',
                    data:['邮件营销','联盟广告']
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '3%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            // fontStyle:"oblique",
                            fontSize:12, 
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    },
                    data: ["1","2","3","4","5","6","7","8"]
                },
                yAxis: {
                    type: 'value',
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    }
                },
                series: [
                    {
                        name:'邮件营销',
                        type:'line',
                        stack: '总量',
                        data:[120, 132, 101, 134, 90, 230, 210,120, 132, 101, 134, 90, 230, 210]
                    },
                    {
                        name:'联盟广告',
                        type:'line',
                        stack: '总量',
                        data:[220, 182, 191, 234, 290, 330, 310,120, 132, 101, 134, 90, 230, 210]
                    },
                ]
            })
            window.onresize = chartsLine_color.resize;
        },
        chartsLine_Bg_right(){//右侧接驳点运力利用率
            var chartsLine_Bg = echarts.init(document.getElementById('chartsLine_Bg_right'));
            chartsLine_Bg.setOption({
                color: ['#2AB7FF'],
                title: {
                    text: '接驳点运力利用率',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                tooltip: {
                    trigger: 'axis'
                },
                legend: {
                    // color:["#ffffff","#000"],
                    icon: "rect",
                    itemWidth:8,//图例的宽度
                    itemHeight:8,//图例的高度
                    textStyle:{//图例文字的样式
                        color:'#b3e3fc',
                        fontSize:12
                    },
                    left:'left',
                    top:'15%',
                    data:['邮件营销','联盟广告']
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '3%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            // fontStyle:"oblique",
                            fontSize:12, 
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    },
                    data: ["1","2","3","4","5","6","7","8"]
                },
                yAxis: {
                    type: 'value',
                    axisLabel:{
                        textStyle:{
                            color:"#849098",
                            
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#fff',
                            width:1,//这里是为了突出显示加上的
                        }
                    }
                },
                series: [
                    {
                        name:'邮件营销',
                        type:'line',
                        stack: '总量',
                        data:[120, 132, 101, 134, 90, 230, 210,120, 132, 101, 134, 90, 230, 210]
                    },
                ]
            })
            window.onresize = chartsLine_Bg.resize;
        },
        chartsBar_right(){//右侧接驳点运力利用率
            var chartsBar_right= echarts.init(document.getElementById('chartsBar_right'));
            chartsBar_right.setOption({
                title: {
                    text: '接驳点运力利用率',
                    left: '3%',
                    top:"3%",
                    textStyle: {
                        color: '#ACDBF4',
                        fontSize: 12,
                    }
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '3%',
                    containLabel: true
                },
                xAxis : [
                    {
                        type : 'category',
                        axisLabel:{
                            textStyle:{
                                color:"#849098",
                            }
                        },
                        axisLine:{
                            lineStyle:{
                                color:'#fff',
                                width:1,//这里是为了突出显示加上的
                            }
                        },
                        data : ['1','2','3','4','5','6','7','8']
                    }
                ],
                yAxis : [
                    {
                        type : 'value',
                        axisLabel:{
                            textStyle:{
                                color:"#849098",
                            }
                        },
                        axisLine:{
                            lineStyle:{
                                color:'#fff',
                                width:1,//这里是为了突出显示加上的
                            }
                        }
                    }
                ],
                series : [
                    {
                        name:'邮件营销',
                        type:'bar',
                        stack: '广告',
                        data:[120, 132, 101, 134, 90, 230, 210]
                    },
                    {
                        name:'联盟广告',
                        type:'bar',
                        stack: '广告',
                        data:[220, 182, 191, 234, 290, 330, 310]
                    },
                    {
                        name:'视频广告',
                        type:'bar',
                        stack: '广告',
                        data:[150, 232, 201, 154, 190, 330, 410]
                    }
                ]
            })
            window.onresize = chartsBar_right.resize;
        },
    },
    created () {
        // axios.get('http://192.168.37.1:8222/parkPedestrianFlowList')
        // .then(function (response) {
        //     console.log(response);
        // })
        // .catch(function (error) {
        //     console.log(error);
        // })
    },
    mounted () {
      this.map();
      this.chartsBar()//左侧第一个Bar
      this.chartsLine()//左侧第二个Line
      this.chartsLine_Bg()//左侧第三个Line

      this.chartsCar()//右侧接驳点运力分配
      this.chartsPie()//右侧接驳点运力缺口
      this.chartsLine_color()//右侧运力变化趋势
      this.chartsLine_Bg_right()//右侧接驳点运力利用率
      this.chartsBar_right()//右侧Bar
    }
}
</script>

<style lang="scss" scoped>
 #index{
    *{margin: 0;padding: 0;}
    #allmap{
        height: 650px;
        overflow: hidden;
    } 
    .leftBox{//右侧图标
        height: 650px;
        background: #0F2E40;
        .tltleText{
            h4{
                color: #93DFF1;
                font-size: 18px;
                padding: 10px 0 0 20px;
            }
        }
        button{
            background:transparent;
            padding: 3px 5px;
            color: #ffffff;
            font-size: 12px;
            border: none;
            outline: none;
            position: absolute;
            top: 10px;
        }
        button.active{ 
            border: none;
            outline: none;               
            border: 1px transparent solid;
            border-image:linear-gradient(to right,#2b88ff,#ffffff) 1 10;
        }
        .yardStop{ right: 30%; }
        .yardStop_no{ right: 15%; }

        .bar{//左侧第一个柱状图
            position: relative;
            #chartsBar{
                width: 380px;
                height: 150px;
                background: #092638;
                margin: 10px 10px;
                // border: 1px solid pink;
            }
        }
        .Line{//左侧第二个折线图
            position: relative;
            #chartsLine{
                width: 380px;
                height: 200px;
                background: #092638;
                margin: 10px 10px;
                // border: 1px solid yellow;
            }
        }
        .Line_Bg{//左侧第三个折线图
            position: relative;
            #chartsLine_Bg{
                width: 380px;
                height: 200px;
                background: #092638;
                margin: 10px 10px;
                // border: 1px solid red;
            }
        }
    }
    .rightBox{//右侧图标
        height: 650px;
        background: #0F2E40;
        #chartsCar, #chartsPie, #chartsLine_color, #chartsLine_Bg_right, #chartsBar_right{
            width: 300px;
            height: 150px;
            background: #092638;
            margin: 10px 10px;
        }
        .carPinCharts,.Line_BgCharts{
            display: flex;
        }
        #chartsBar_right{width: 520px;height: 250px}
    } 
 }
</style>
