<template>
    <div class="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
        <scroll class="content">
            <detail-swiper :top-images="topImages" ></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
        </scroll>

    </div>
</template>
<script>
import DetailSwiper from './childComps/DetailSwiper'
import DetailNavBar from './childComps/DetailNavBar'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo.vue'
import {getDetail,Goods,Shop} from 'network/detail'
import Scroll from 'components/common/scroll/Scroll'
export default {
    name:'Detail',
    components:{
        DetailNavBar,
        DetailSwiper,
        DetailBaseInfo,
        DetailShopInfo,
        Scroll
    },
    data() {
        return {
            iid:null,
            topImages:[],
            goods:{},
            shop:{},
            detail
        }
    },
    created(){
        this.iid=this.$route.params.iid
        getDetail(this.iid).then(res=>{
            const data=res.result
            console.log(res)
            this.topImages=data.itemInfo.topImages
            this.goods=new Goods (data.itemInfo,data.columns,data.shopInfo.services)
            // console.log(this.goods)
            this.shop=new Shop(data.shopInfo)
            console.log(this.shop)
            
        })
    }
}
</script>
<style scoped>
.detail{
    background-color: #fff;
    z-index: 9;
    height: 100vh;
}
.detail-nav{
    position: relative;
    z-index: 9;
    background-color: #fff;
}
.content{
    height: calc(100%-44px);
    overflow: hidden;
}
</style>