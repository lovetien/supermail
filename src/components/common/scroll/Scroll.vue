<template>
    <div class="wrapper" ref="wrapper">
        <div class="content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    import BScroll from 'better-scroll'
    export default {
        name:"Scroll",
        props:{
            probeType:{
                type: Number,
                default: 0
            },
            pullUpLoad:{
                type: Boolean,
                default: false
            }
        },
        data(){
            return{
                scroll:null
            }
        },
        mounted(){
            //创建BScroll对象
            this.scroll = new BScroll(this.$refs.wrapper,{
                click: true,
                probeType:this.probeType,
                pullUpLoad:this.pullUpLoad
            })
            //监听滚动的位置
            if(this.probeType === 2 || this.probeType === 3){
                this.scroll.on('scroll',(position)=>{
                    this.$emit('scroll',position)
                })
            }
           
            //监听scroll滚动到底部
            if(this.pullUpLoad){
                this.scroll.on('pullingUp',()=>{
                    this.$emit('pullingUp')
                })
            }
        },
        methods:{
            scrollTo(x,y,time=300){
               this.scroll && this.scroll.scrollTo(x,y,time)
            },
            finishPullUp(){
                this.scroll.finishPullUp()
            },
            refresh(){
                this.scroll && this.scroll.refresh()
            }
        }
    }
</script>

<style  scoped>

</style>