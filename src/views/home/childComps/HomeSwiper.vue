<template>
    <div>
        <swiper>
            <swiper-item v-for="item in banners" :key="item.id">
                <a :href="item.link">
                    <img :src="item.image" alt="" @load="imageLoad"> 
                </a>
            </swiper-item>
        </swiper>
    </div>
        
</template>

<script>
    import {Swiper,SwiperItem} from 'components/common/swiper'
    export default {
        name:"HomeSwiper",
        props:{
            banners:{
                type:Array,
                default(){
                    return []
                }
            }
        },
        data(){
            return {
                isload: false
            }
        },
        components:{
            Swiper,
            SwiperItem
        },
        methods:{
            imageLoad(){
                if(this.isload){
                    this.$emit('swiperImageLoad')
                    this.isload = true
                }
               
            }
        }
    }
</script>

<style scoped>

</style>