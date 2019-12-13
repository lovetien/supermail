<template>
    <div>
        <swiper>
            <swiper-item v-for="item in banners" :key="item.id">
                <a :href="item.link">
                    <img :src="item.image" alt="1213"> 
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
        components:{
            Swiper,
            SwiperItem
        }
    }
</script>

<style scoped>

</style>