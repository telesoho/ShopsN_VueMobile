<template>
    <div class="teacher-main">
        <header class="header">
            <span @click="btnGo" class="btnGo"></span>
            {{text}}
            <div class="integral-title" v-if="title" @click="screen">{{title}}<i class="icon"></i></div>
            <div class="input-main fl" v-if="sea"><input type="text" placeholder="请输入商品名称..."></div>
            <span class="cartBtn" v-if="$store.state.catr_number" @click="toCart"><em>{{$store.state.catr_number}}</em></span>
            <div class="btn-search" v-if="search"></div>
            <div class="btn-ng clearfix" v-if="!btn" v-show="!set" @click="addClass">
                <span class="fl"></span>
                <span class="fl"></span>
                <span class="fl"></span>
            </div>
            <transition  name="fade">
                <div class="box-wrap" v-if="sta">
                    <div class="box-bg" @click="addClass"></div>
                    <div class="bomb-box">
                        <div class="polygon"></div>
                        <div class="btn" @click="toLink('/home')">
                            <span class="icon"><i></i></span>首页
                        </div>
                        <div class="btn">
                            <span class="icon" @click="toLink('/myNews')"><i></i></span>消息
                        </div>
                    </div>
                </div>
            </transition>
            <div class="rule" v-if="rule" @click="addShow">规则</div>
            <div class="service" v-if="btn">{{btn}}</div>
            <div class="btn-delete" v-if="btnDel" @click="btndl"></div>
            <div class="btn-del" v-if="delData" @click="del"></div> 
        </header>
        <div class="hd-active"></div>
    </div>
</template>
<script>
    import qs from 'qs';
    import { MessageBox,Toast } from 'mint-ui';
    export default {
        name : 'header',
        data(){
            return {
                sta:false,
                ruleCon:null
            }
        },
        props:{
            text:null,
            // number:null,
            btn:null,
            search:null,
            rule:null,
            btnDel:null,
            sea:null,
            set:null,
            title:'',
            id:'',
            delData:null,
            fooptStatus:null,
            checklist:''
        },
        computed:{
            popupVisible(){
                return this.popupVisible;
            }
        },
        methods:{
            btnGo(){
                this.$router.go(-1);
            },
            toCart(){
                sessionStorage.setItem('router_index', 2);
                this.$router.push({
                    path:'/cart'
                });
            },
            toLink(link){
                this.$router.push({
                    path:link
                });
            },
            btndl(){
                MessageBox.confirm('确定执行此操作?').then(action => {
                    this.axios.post(API_URL + 'Home/Pcenter/addressde',qs.stringify({
                        app_user_id:sessionStorage.getItem('user_ID'),
                        id:this.id
                    })).then((res) => {
                        Toast(res.data.msg);
                        this.$router.push('/address')
                    }).catch((err) => {
                        console.log(err);
                    });
                });
            },
            del(){
                MessageBox.confirm('确定清空所有足迹?').then(action => {
                    this.axios.post(API_URL + 'Home/Pcenter/deleteFootprint').then((res) => {
                        this.$store.state.footprint = '';
                        Toast('成功清除所有足迹快去逛逛吧！');
                    }).catch((err) => {
                        console.log(err);
                    });
                }).catch((err) => {
                    console.log(err);
                });
            },
            addClass(){
                if(this.sta == false){
                    this.sta = true;
                }else{
                    this.sta = false;
                }
            },
            addShow(){
                this.ruleCon = true;
                this.$emit('addShow')
            },
            screen(){
                this.popupVisible = true;
                this.$emit('screen');
            }
        }
    }
</script>
<style lang="less" scoped>
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s ;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0
    }
    
    .hd-active{
        width: 100%;
        height: .62rem;
        padding: .16rem 0;
    }
    .header {
        width: 100%;
        height: .62rem;
        background: #d0111b;
        padding: .16rem 0;
        text-align:center;
        position:relative;
        color:#fff;
        font-size:.36rem;
        font-weight:bold;
        line-height:.62rem;
        position:fixed;
        top:0;
        left:0;
        z-index:1;
        .integral-title{
            font-size: .32rem;
            color:#fff;
            .icon{
                display:inline-block;
                width: .27rem;
                height: .16rem;
                background:url(../../../assets/bottom-btn-icon.png) no-repeat;
                background-size:100% 100%;
                margin-left:.1rem;
            }
        }
        .input-main {
            width: 5.4rem;
            height: 100%;
            border-radius: 100px;
            background: rgba(255, 255, 255, .3) url(../../../assets/search1.png) no-repeat .24rem center;
            background-size: .26rem .27rem;
            line-height:100%;
            margin-left:1rem;
            input {
                height: 100%;
                width: 100%;
                border: none;
                background: none;
                text-indent: .7rem;
                font-size: .26rem;
                color: #fff;
                outline: none;
            }
            input::-webkit-input-placeholder, textarea::-webkit-input-placeholder { 
                color: #fff; 
            } 
            input:-moz-placeholder, textarea:-moz-placeholder { 
                color: #fff; 
            } 
            input::-moz-placeholder, textarea::-moz-placeholder { 
                color: #fff; 
            } 
            input:-ms-input-placeholder, textarea:-ms-input-placeholder { 
                color: #fff; 
            } 
        }
        .rule{
            position:absolute;
            right:1rem;
            top:0;
            height: 100%;
            line-height:1rem;
            font-size:.28rem;
        }
        .rule:active{
            text-shadow: 0 0 5px #ccc;
        }
        .box-wrap{
            position:fixed;
            width: 100%;
            height: 100%;
            left:0;
            top:0;
            z-index:99;
            .box-bg{
                position:absolute;
                left:0;
                top:0;
                width: 100%;
                height: 100%;
                z-index:1;
            }
            .bomb-box{
                background:#535353;
                padding:.11rem;
                box-sizing: border-box;
                position: absolute;
                right:.15rem;
                top:.8rem;
                border-radius:5px;
                z-index:2;
                .polygon{
                    width: .4rem;
                    height: .4rem;
                    position:absolute;
                    top:0;
                    left:.8rem;
                    transform: rotate(45deg);
                    background:#535353;
                }
                .btn:nth-child(2){
                    border-bottom:1px solid #666;
                    .icon{
                        i{
                            width: .24rem;
                            height: .24rem;
                            background:url(../../../assets/home.png) no-repeat;
                            background-size:100% 100%;
                        }
                    }
                }
                .btn:nth-child(3){
                    border-top:1px solid #444;
                    .icon{
                        i{
                            width: .24rem;
                            height: .24rem;
                            background:url(../../../assets/news1.png) no-repeat;
                            background-size:100% 100%;
                        }
                    }
                }
                .btn{
                    width: 1.1rem;
                    height: .56rem;
                    box-sizing: border-box;
                    text-align:center;
                    line-height: .56rem;
                    position:relative;
                    padding-left:.5rem;
                    font-size:.24rem;
                    .icon{
                        position:absolute;
                        left:0;
                        top:0;
                        width: .56rem;
                        height: 100%;
                        margin:0;
                        border:none;
                        background:none;
                        i{
                            position:absolute;
                            left:0%;
                            top:0%;
                            bottom:0;
                            right:0;
                            margin:auto;
                        }
                    }
                }
            }
        }
        .btnGo{
            position:absolute;
            left:.2rem;
            top:50%;
            margin-top:-.23rem;
            width:.46rem;
            height:.46rem;
            background:url(../../../assets/btn-return.png) no-repeat;
            background-size:100% 100%;
        }
        .cartBtn{
            position:absolute;
            right:1.2rem;
            top:50%;
            margin-top:-.23rem;
            width:.6rem;
            height:.46rem;
            background:url(../../../assets/cart.btn.png) no-repeat;
            background-size:100% 100%;
            em{
                position:absolute;
                right:-.1rem;
                top:-.05rem;
                background:#ff883f;
                color:#fff;
                font-size:.21rem;
                font-style:normal;
                height:.33rem;
                line-height:.23rem;
                padding:.05rem;
                border-radius:100%;
                box-sizing:border-box;
                min-width:.33rem;
            }
        }
        .service{
            line-height:.62rem;
            position:absolute;
            top:50%;
            right:.2rem;
            margin-top:-.31rem;
            font-size:.2rem;
            color:#fff;
        }
        .btn-ng{
            position:absolute;
            top:0;
            right:.2rem;
            height: 100%;
            span{
                display:block;
                width:.12rem;
                height:.12rem;
                border:.06rem solid #fff;
                border-radius:50%;
                box-sizing:border-box;
                background:#fd4f4b;
                margin-left:.1rem;
                margin-top:.44rem;
            }
        }
        .btn-search{
            width:.44rem;
            height:.45rem;
            position:absolute;
            top:50%;
            right:1rem;
            background:url(../../../assets/search.png) no-repeat;
            background-size:100% 100%;
            margin-top:-.225rem;
        }
        .btn-delete{
            position:absolute;
            top:50%;
            right:.2rem;
            margin-top:-.205rem;
            width: .41rem;
            height: .41rem;
            background:url(../../../assets/delete.png) no-repeat;
            background-size:100% 100%;
        }
        .btn-del{
            position:absolute;
            top:50%;
            right:1rem;
            margin-top:-.205rem;
            width: .41rem;
            height: .41rem;
            background:url(../../../assets/delete.png) no-repeat;
            background-size:100% 100%;
        }
        .btn-del.active{
            background:none;
            width: .6rem;
            margin-top:-.3rem;
        }
    }
</style>