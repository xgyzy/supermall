<template>
<div id="home">
    <nav-bar class="home-nav">
        <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
</div>
</template>
<script>
import NavBar from 'components/common/navbar/NavBar'
import HomeSwiper from './childComps/HomeSwiper'
import {getHomeMultidata} from 'network/home'
import RecommendView from './childComps/RecommendView.vue'
import FeatureView from './childComps/FeatureView.vue'

export default {
    name:"Home",
    components:{
        NavBar,
        getHomeMultidata,
        HomeSwiper,
        RecommendView,
        FeatureView
    },
    data() {
        return {
            banners:[],
            recommends:[]
        }
    },
    created(){
        getHomeMultidata().then(res=>{
            // this.result=res
            this.banners =res.data.banner.list
            this.recommends=res.data.recommend.list
            console.log(this.recommends)
        })
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
</style>