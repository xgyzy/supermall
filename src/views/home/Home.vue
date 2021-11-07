<template>
<div id="home">
    <nav-bar class="home-nav">
        <div slot="center">购物街</div>
    </nav-bar>
        <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll" :pullUpLoad="true" @pullingUp="loadMore">
            <home-swiper :banners="banners" ></home-swiper>
            <recommend-view :recommends="recommends"></recommend-view>
            <feature-view></feature-view>
            <tab-control  :titles="['流行','新款','精选']" @tabClick="tabClick" ref="tabControl"></tab-control>
            <goods-list :goods="goods[currentType].list"></goods-list>
        </scroll>
        <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
</div>
</template>
<script>
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import BackTop from 'components/content/backTop/BackTop'

import NavBar from 'components/common/navbar/NavBar'
import Scroll from 'components/common/scroll/Scroll'
import {debounce} from 'components/common/utils'

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
        Scroll,
        BackTop,

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
            },
            currentType:'pop',
            isShowBackTop:false,
            tabOffsetTop:0
        }
    },
    created(){
        this.getHomeMultidata()
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')

    },
    // updated(){
    //             console.log(this.$refs.tabControl.$el.offsetTop)

    // },
    mounted(){
        // this.$refs.swiper
        const refresh=debounce(this.$refs.scroll.refresh,50)
        this.$bus.$on('itemImageLoad',()=>{
                refresh()
        })
    },
    methods:{
        //事件监听========================================================
        // loadMore(){
        // this.getHomeGoods(this.currentType)
        // this.$refs.scroll.scroll.refresh()
        // },
        loadMore(){
            this.getHomeGoods(this.currentType)
        },
        contentScroll(position){
            // console.log(position)
            this.isShowBackTop=-(position.y)>1000
        },
        backClick(){
            this.$refs.scroll.scrollTo(0,0)
        },
        tabClick(index){
            switch (index){
                case 0:
                    this.currentType='pop'
                    break
                case 1:
                    this.currentType='new'
                    break
                case 2:
                    this.currentType='sell'
            }
        },


        //网络请求========================================================
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
                this.$refs.scroll.finishPullUp()
            })
        }
    }
}
</script>
<style scoped>
#home{
    padding-top: 44px;
    height: 100vh;
    position: relative;
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

.content{
    position: absolute;
    left: 0;
    right: 0;
    
    bottom: 49px;
    top: 44px;
    overflow: hidden;
}
</style>