<template>
    <div class="wrapper" ref="wrapper">
        <div class="content">
            <slot>
            </slot>
        </div>
        
    </div>
</template>
<script>
import BScroll from 'better-scroll'
export default {
    name:"Scroll",
           props:{
               probeType:{
                type:Number,
                default:0
               },
               pullUpLoad:{
                   type:Boolean,
                   default:false
               }
           },
    data() {
        return {
           scroll:null,
        };
    },
    mounted(){
        this.scroll=new BScroll(this.$refs.wrapper,{
            click:true,
            probeType:this.probeType,
            pullUpLoad:this.pullUpLoad

        })
        if(this.probeType ===2 || this.probeType ===3){
            this.scroll.on('scroll',(position)=>{
            // console.log(position)
            this.$emit('scroll', position);
        })
        }
        if(this.pullUpLoad){
            this.scroll.on('pullingUp',()=>{
            this.$emit('pullingUp');
            // console.log("123")
        })
        }
    },
    methods:{
        scrollTo(x,y,time=300){
            this.scroll && this.scroll.scrollTo(x,y,time)
        },
        finishPullUp(){
            this.scroll && this.scroll.finishPullUp()
        },
        refresh(){
            // console.log('111')
            this.scroll && this.scroll.refresh()
        }
    }
}
</script>

<style scoped>

</style>