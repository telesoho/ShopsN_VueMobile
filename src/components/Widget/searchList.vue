<template>
    <div>
        <ul class="list-wrap" v-if="data">
            <li class="clearfix" v-for="(item,index) in data" :key="item.id" @click="tolink(item)">
                <img v-lazy="URL + item.pic_url" class="fl">
                <div class="list-text fl">
                    <p class="text">{{item.title}}</p>
                    <p class="new-price" v-if="integral"><span>{{item.integral}}&nbsp;<em>积分</em></span></p>
                    <p class="new-price" v-if="!integral">￥<span>{{item.price_market}}</span></p>
                    <p class="status" v-if="!integral"><span>已有{{item.comment}}条评论</span><span>{{item.trade}}笔交易成功</span></p>
                    <p class="status" v-if="integral" style="padding-top:.2rem;"><span>市场参考价：{{item.price_market}}元</span></p>
                </div>
            </li>
        </ul>
        <div class="text-center status" v-if="!data">没有此类商品！</div>

        <div class="load" v-show="load" @touchmove.prevent><mt-spinner type="triple-bounce" color="rgb(38, 162, 255)"></mt-spinner></div>
    </div>
</template>
<script>
    export default {
        name:'searchList',
        data(){
            return {
                load_wrap:true
            }
        },
        props:{
            data:'',
            integral:'',
            load:''
        },
        watch:{
            integral(){
                return this.integral
            }
        },
        methods:{
            tolink(item){
                if(item.goods_id){
                    this.$router.push({
                        name:'product',
                        params:{
                            id:item.goods_id,
                            status:2
                        }
                    });
                }else{
                    this.$router.push({
                        name:'product',
                        params:{
                            id:item.id,
                            status:1
                        }
                    });
                }
                
            }
        }
    }
</script>
<style lang="less" scoped>
    .status{
        padding-top:.2rem;
        font-size:.3rem;
        color:#333;
    }
    .list-wrap{
        padding-top:.3rem;
        background:#fff;
        li{
            padding:.13rem;
            height:2.2rem;
            border-bottom:1px solid #cbcbcb;
            img{
                width:2.2rem;
                height:2.2rem;
            }
            .list-text{
                width:4.3rem;
                padding-left:.25rem;
                .text{
                    font-size:.32rem;
                    color:#333;
                    line-height:.45rem;
                    padding-top:.1rem;
                    height:.9rem;
                    overflow:hidden; 
                    text-overflow:ellipsis;
                    display:-webkit-box; 
                    -webkit-box-orient:vertical;
                    -webkit-line-clamp:2; 
                }
                .new-price{
                    font-size:.32rem;
                    color:#f23030;
                    padding-top:.3rem;
                    span{
                        font-size:.4rem;
                        font-weight:bold;
                        em{
                            font-style:normal;
                            font-size:.25rem;
                            color:#7c7c7d;
                            line-height: .25rem;
                        }
                    }
                }
                .status{
                    color:#ddd;
                    padding-top:.1rem;
                    span{
                        font-size:.24rem;
                    }
                    span:nth-child(2){
                        padding-left:.3rem;
                    }
                }
            }
        }
    }
</style>