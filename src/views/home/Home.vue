<template>
<div id="home">
    <nav-bar class="home-nav">
        <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
    <tab-control class="tab-control" :titles="['流行','新款','精选']"></tab-control>
    <goods-list :goods="goods['pop'].list"></goods-list>
</div>
</template>
<script>
import GoodsList from 'components/content/goods/GoodsList'
import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'

import {getHomeMultidata,getHomeGoods} from 'network/home'

import RecommendView from './childComps/RecommendView.vue'
import FeatureView from './childComps/FeatureView.vue'
import HomeSwiper from './childComps/HomeSwiper'
export default {
    name:"Home",
    components:{
        GoodsList,
        NavBar,
        TabControl,

        getHomeMultidata,
        getHomeGoods,

        HomeSwiper,
        RecommendView,
        FeatureView
    },
    data() {
        return {
            banners:[],
            recommends:[],
            goods:{
                'pop':{page:0,list:[]},
                'new':{page:0,list:[]},
                'sell':{page:0,list:[]},
            }
        }
    },
    created(){
        this.getHomeMultidata()
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
    },
    methods:{
        getHomeMultidata(){
            getHomeMultidata().then(res=>{
            // this.result=res
            this.banners =res.data.banner.list
            this.recommends=res.data.recommend.list
            console.log(this.recommends)
        })
        },
        getHomeGoods(type){
            const page=this.goods[type].page+1
            getHomeGoods(type,page).then(res=>{
                console.log(res);
                this.goods[type].list.push(...res.data.list)
                this.goods[type].page+=1
            })
        }
    }
}
</script>
<style scoped>
#home{
    padding-top: 44px;
}
.home-nav{
    background-color: var(--color-tint);
    position: fixed;
    color: #fff;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
}
.tab-control{
    /* 粘性属性 */
    position:sticky;
    top: 44px;
}
</style>