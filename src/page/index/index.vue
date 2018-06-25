<template>
   <div>
       <div class="header">
            <div class="back iconfont">&#xe624;</div> 
            <div class="search">输入城市/景点/游玩主题</div>
            <div class="city">
                城市
                <i class="iconfont">&#xe791;</i>    
            </div>
        </div>
        <div class="swiper-img-con">
            <div class="swiper-container">
                <div class="swiper-wrapper">
                    <div class="swiper-slide" v-for="item in swiperInfo" :key="item.id">
                        <div> 
                            <img class="swiper-img" :src="item.imgUrl" alt="" /> 
                        </div> 
                    </div> 
                </div>
                <div class="swiper-pagination"></div>
            </div>
        </div>  
        <div>
            <div class="swiper-container-list">
                <div class="swiper-wrapper">
                    <div class="swiper-slide" v-for="(page,index) in pages" :key="index">
                        <div class="swiper-icon" v-for="item in page" :key="item.id"> 
                            <img :src="item.imgUrl" alt="" /> 
                        </div> 
                    </div> 
                </div>
            </div>
        </div>  
    </div> 
</template>

<script>
import Swiper from 'swiper'
export default {
    data(){
        return {
            swiperInfo:[],
            iconsInfo:[]
        }
    },
    created(){
        this.swipergetData()
    },
    updated(){
       this.swiper()
       this.swiperlist()
    },
    computed:{
        pages(){
            console.log(this.iconsInfo)
            let icons = [];
            this.iconsInfo.forEach((val,index) => {
               let page = Math.floor(index/8)         
               if(!icons[page]){
                   icons[page] = []
               }
               icons[page].push(val)
            })
            return icons
        }
    },
    methods:{
        swiper(){
            var swiper = new Swiper('.swiper-container',{
                 pagination: {
                    el: '.swiper-pagination',
                },
            });
        },
        swiperlist(){
            var swiper = new Swiper('.swiper-container-list',{
            });
        },
        swipergetData(){
            this.$http.get('/static/data/index.json')
                .then(this.handleGetDataSucc.bind(this))
        },
        handleGetDataSucc(res){
            const body = res.body
            if(body && body.data && body.data.swiper){
                this.swiperInfo = body.data.swiper
                this.iconsInfo = body.data.icons
            }
            
        }
    }
    
}
</script>

<style scoped>
    .header{
        display:flex;
        background:#05bad5;
        color:#fff;
        font-size:.12rem;
    }
    .back{
        width:.64rem;
        line-height:.86rem;
        text-align:center;
    }
    .search{
        flex:1;
        background: #fff;
        margin:.09rem .08rem;
        border-radius:.1rem;
        color:#e4e7ea;
        line-height:.68rem;
        padding-left:.1rem;
    }
    .city{
        width:1.32rem;
        line-height:.86rem;
    }
    .swiper-img-con{
        overflow:hidden;
        width:100%;
        height:0;
        padding-bottom:26.66%;
    }
    .swiper-img{
        width:100%;
    }
    .swiper-icon{
        width:25%;
        float:left;
    }
    .swiper-icon img{
        width:100%;
    }
</style>