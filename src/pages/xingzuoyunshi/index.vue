<template>
    <div class="xingzuo">
		<c-header :title="title"></c-header>
        <div class="flex  showAll pad20">
            <div  class="title">
			    {{title}}
		    </div>
            <div class="content flex">
                <div @click="getYunshi(item.astroid,item.color)" v-for="(item) of xzList" :key="'xz'+item.astroid" class="xzItem flex">
                    <img :src="item.pic" :alt="item.astroname">
                    <p :style="{'color':item.color}">{{item.astroname}}</p>
                    <p :style="{'color':item.color}">{{item.date}}</p>
                </div>
            </div>
            <div class="answer" v-show="showFlag">
                <div class="atitle" :style="{'background':titleColor}">{{ysData&&ysData.astroname}}的运势如下</div>
                <div class="descrip">
                    <el-collapse  v-model="activeName" accordion>
                        <el-collapse-item  :title="ysData&&ysData.today.date+'日(今天)'" name="1">
                            <div class="center flex">
                                <div class="today" style="width: 300px;height:300px;"></div>
                                <div>幸运数字：{{ysData&&ysData.today.number}}</div>
                                <div>幸运色：{{ysData&&ysData.today.color}}</div>
                                <div>匹配星座：{{ysData&&ysData.today.star}}</div>
                                <div>总结：{{ysData&&ysData.today.presummary}}</div>
                            </div>
                        </el-collapse-item>
                        <el-collapse-item  :title="ysData&&ysData.tomorrow.date+'日(明天)'" name="2">
                            <div class="center flex">
                                <div class="tomorrow" style="width: 300px;height:300px;"></div>
                                <div>幸运数字：{{ysData&&ysData.tomorrow.number}}</div>
                                <div>幸运色：{{ysData&&ysData.tomorrow.color}}</div>
                                <div>匹配星座：{{ysData&&ysData.tomorrow.star}}</div>
                                <div>总结：{{ysData&&ysData.tomorrow.presummary}}</div>
                            </div>
                        </el-collapse-item>
                        <el-collapse-item :title="ysData&&ysData.week.date+'日(本周)'" name="3">
                            <div>事业：{{ysData&&ysData.week.career}}</div>
                            <div>爱情：{{ysData&&ysData.week.love}}</div>
                            <div>健康：{{ysData&&ysData.week.health}}</div>
                            <div>财运：{{ysData&&ysData.week.money}}</div>
                        </el-collapse-item>
                        <el-collapse-item :title="ysData&&ysData.month.date+'月(本月)'" name="4">
                           <div>事业：{{ysData&&ysData.month.career}}</div>
                            <div>爱情：{{ysData&&ysData.month.love}}</div>
                            <div>健康：{{ysData&&ysData.month.health}}</div>
                            <div>财运：{{ysData&&ysData.month.money}}</div>
                            <div>总结：{{ysData&&ysData.month.summary}}</div>
                        </el-collapse-item>
                        <el-collapse-item :title="ysData&&ysData.year.date+'年(今年)'" name="5">
                            <div>事业：{{ysData&&ysData.month.career}}</div>
                            <div>爱情：{{ysData&&ysData.month.love}}</div>
                            <div>财运：{{ysData&&ysData.month.money}}</div>
                            <div>总结：{{ysData&&ysData.month.summary}}</div>
                        </el-collapse-item>
                    </el-collapse>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import header from 'components/header'
    export default {
        components:{
            'c-header': header
        },
        data(){
            return {
                title:"星座运势",
                stitle:"查看星座",
                xzList:[],
                appKeyList:['838a3a3dafd35efd','641165fb206f865e','64da13f43137e334','4a9890730a15b7b4'],
                appKey:'4a9890730a15b7b4',
                keyIndex:0,
                url:'http://fkchen.com:7677/api/astro',
                date:'',
                ysData:null,
                count:0,
                showFlag:false,
                activeName: '1',
                titleColor:'#ff0008',
            }
        },
        methods:{
            async getYunshi(aid,color){
                this.confirm=1;
                let result = await this.axios.get(`${this.url}?astroid=${aid}`);
                let dataObj=result.data;
                // console.log(dataObj);
                if(dataObj){
                    this.titleColor=color;
                    this.ysData = dataObj;
                    this.setcolor(color);
                    this.showFlag = true;
 
                }else{
                    // this.count++;
                    // if(this.count===10){
                    //     this.count=0;
                    //     return;
                    // }
                    // this.keyIndex = ++this.keyIndex>=this.appKeyList.length?0:this.keyIndex;
                    // this.appKey=this.appKeyList[this.keyIndex];
                    // // console.log(this.appKey,this.keyIndex);
                    // this.getYunshi(aid,color);
                }  
                
            },
            setcolor(color){
                let head = document.querySelectorAll('.el-collapse-item__header');
            
                // console.log(head);
                for(let item of head){
                    item.style.color=color;
                }
                this.setEcharts(color);
            },
            setEcharts(color){
                let today = this.echarts.init(document.querySelector('.today'));
                let tomorrow = this.echarts.init(document.querySelector('.tomorrow'));
                let todayData=this.ysData.today;
                let tomorrowData=this.ysData.tomorrow;
                let option1 = {  
                    title: {
                        text: '今日指数'
                    },
                     
                    tooltip: {},
                    radar: {
                        // shape: 'circle',
                        name: {
                            textStyle: {
                                color: '#fff',
                                backgroundColor: color,
                                borderRadius: 3,
                                padding: [3, 5]
                        }
                        },
                        indicator: [
                            { name: '健康', max:5 },
                            { name: '财运', max:5 },
                            { name: '事业', max:5 },
                            { name: '爱情', max:5 },
                            { name: '总计', max:5 },
                        ]
                    },
                    series: [{
                        name: '今日指数',
                        type: 'radar',
                        itemStyle: {normal: {areaStyle: {type: 'default'}}},
                        data : [
                            {
                                value : [todayData.health, todayData.money, todayData.career, todayData.love, todayData.summary],
                                name : '今日指数）'
                            }
                        ]
                    }]
                };
                
                let option2 = {  
                    title: {
                        text: '明日指数'
                    },
                     
                    tooltip: {},
                    radar: {
                        // shape: 'circle',
                        name: {
                            textStyle: {
                                color: '#fff',
                                backgroundColor: color,
                                borderRadius: 3,
                                padding: [2, 2]
                        }
                        },
                        indicator: [
                            { name: '健康', max:5 },
                            { name: '财运', max:5 },
                            { name: '事业', max:5 },
                            { name: '爱情', max:5 },
                            { name: '总计', max:5 },
                        ]
                    },
                    series: [{
                        name: '明日指数',
                        type: 'radar',
                        itemStyle: {normal: {areaStyle: {type: 'default'}}},
                        data : [
                            {
                                value : [tomorrowData.health, tomorrowData.money, tomorrowData.career, tomorrowData.love, tomorrowData.summary],
                                name : '明日指数）'
                            }
                        ]
                    }]
                };
                today.setOption(option1);
                tomorrow.setOption(option2);
            }
        },
        computed:{

        },
        mounted(){
            this.appKey = this.appKeyList[this.keyIndex]
        },
        updated(){

        },
        async created(){
            let result = await this.axios.get('/static/json/constellation.json');
            // console.log(result.data);
            this.xzList=result.data.list;
            this.stitle=result.data.title;
            this.title=result.data.name;
        }
    }
</script>
<style lang="scss" scoped>
    .flex{
        display: flex;
        justify-content: space-around;
        align-items: center;
        flex-wrap: wrap;
    }
    .xingzuo{
        .showAll{
            margin-bottom: 0.625rem;
            flex-direction: column;
            .title {
                font-size: 0.46875rem;
                font-weight: 600;
                color: #000000;
                margin: 0.625rem 0;
                text-align: center;
            }
            .content{
                border: 1px solid #850004;
                padding: 0.3125rem;
                width: 100%;
                .xzItem{
                    flex-direction: column;
                    width: 130px;
                    padding: 10px 0;
                    
                }
            }
            .answer{
                margin-top: 0.46875rem;
                width: 100%;
                .atitle{
                    width: 100%;
                    height: 1.25rem;
                    background: #ff0008;
                    text-align: center;
                    line-height: 1.25rem;
                    font-size: 0.46875rem;
                    color: white;
                }
                .descrip{
                    padding: 0.3125rem;
                    border: solid 0.015625rem #cccccc;
                    margin-bottom: 0.625rem;
                    .center{
                        flex-direction: column;
  
                    }
                    div{
                        padding: 10px 0;
                    }
                }
            }
        }
        
    }

    
</style>