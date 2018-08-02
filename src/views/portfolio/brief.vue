<style type="text/css">
    .test{
        margin: 20px 20px 20px 20px;     
    }
    .test Button{
        margin-bottom: 10px;
    }
</style>
<!--<template>
    <div class="test">
        <Button type="warning" @click="click()">test</Button>
        <Button type="warning" @click="click2()">测试</Button>
        <Input v-model="value" type="textarea" :rows="4" placeholder="Enter something..."></Input>
    </div>
</template>
<script>
/*    import Drilldown from '../node_modules/highcharts/modules/Drilldown.js'
    import Highcharts from 'highcharts'
    Drilldown(Highcharts);

    <vue-highcharts :highcharts="Highcharts" :options="drilldownOptions" ref="drilldownChart"></vue-highcharts>*/

    export default {
        data(){
            return {
                value: null
            }
        },
        methods: {
            click(){
                this.$store.dispatch('userLogin',{"user_name":"test1","user_password":"123","router":this.$router});
                this.$router.push({ path: 'base' }) ;
            },
            click2(){
                this.axios({
                    /*headers: {'Authorization': 'bearer '+this.$store.state.users.currentUser.UserToken},*/
                    method: 'post',
                    url: '/test',
                    data: {
                        "test": "123456"
                    }
                }).then(function(response){
                    /*console.log(response);*/
                    this.value = response.data;
                }.bind(this)).catch(function(error){
                    console.log(error);
                });
            }
        }   
    };
</script>-->
<template>
    <div>
        <vue-highcharts :options="options" ref="lineCharts"></vue-highcharts>
        <button @click="load">load</button>
    </div>
</template>

<script>
    import VueHighcharts from 'vue2-highcharts'
/*    const asyncData = {
        name: 'Tokyo',
        data: [7.0, 6.9, 9.5, 14.5, 18.2, 21.5, 25.2, 26.5, 23.3, 18.3, 13.9, 9.6]
    }*/

    const asyncData = [{"bid_date":"2018-07-01","bid_count":19,"bid_total_amount":3249},
        {"bid_date":"2018-07-05","bid_count":25,"bid_total_amount":3875},
        {"bid_date":"2018-07-10","bid_count":38,"bid_total_amount":3205},]

/*    const asyncData = {
        name: 'bid_list_summary',
        bid_date: ["2018-7-1","2018-7-5","2018-7-10"],
        bid_total_amount: [19,23,25]
    }*/

    export default{
        components: {
            VueHighcharts
        },
        data(){
            return{
                options: {
                    chart: {
                        type: 'spline'
                    },
                    title: {
                        text: 'Monthly Average Temperature'
                    },
                    subtitle: {
                        text: 'Source: WorldClimate.com'
                    },
                    xAxis: {
                        categories: ['Jan', 'Feb', 'Mar']
                    },

/*                    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'*/
                    yAxis: {
                        title: {
                            text: 'Temperature'
                        },
                        labels: {
                            formatter: function () {
                                return this.value + '°';
                            }
                        }
                    },
                    tooltip: {
                        crosshairs: true,
                        shared: true
                    },
                    credits: {
                        enabled: false
                    },
                    series: []
                }
            }
        },
        methods: {
            load(){
                let lineCharts = this.$refs.lineCharts;
                lineCharts.delegateMethod('showLoading', 'Loading...');
                setTimeout(() => {
                    let bidListData = asyncData;
                    let length = bidListData.length;

                    let blcategories = [];
                    let blseries = [];
                    for(let i=0;i<length;i++){

                        blcategories.push(bidListData[i]['key']);
                        blseries.push({'y':bidListData[i]['value'],'name':bidListData[i]['key']+'%'});
                    }
                    lineCharts.addSeries(blseries);
                    lineCharts.hideLoading();

                }, 2000)


            }
        }
    }
</script>