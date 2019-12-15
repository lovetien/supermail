<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
        <tab-control ref="tabControl1"
                     :titles="['流行','新款','精选']"
                     @tabClick="tabClick"
                     class="tab-control"
                     v-show="isTabFixed" />
        <scroll class="content" 
                ref="scroll" :probe-type="3" 
                :pull-up-load="true" @scroll="contentScroll"
                @pullingUp="loadMore">
            <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
            <recommend-view :recommends="recommends" />
            <feature-view />
            <tab-control 
                        ref="tabControl2"
                        :titles="['流行','新款','精选']"
                        @tabClick="tabClick" />
            <goods-list :goods="showGoods" />
        </scroll>
        <back-top @click.native="backClick" v-show="isShowBackTop"/>
    </div>
</template>

<script>
    import HomeSwiper from './childComps/HomeSwiper';
    import RecommendView from './childComps/RecommendView';
    import FeatureView from './childComps/FeatureView';
    
    import NavBar from 'components/common/navbar/NavBar';
    import TabControl from 'components/content/tabControl/TabControl'
    import GoodsList from 'components/content/goods/GoodsList'
    import Scroll from 'components/common/scroll/Scroll'
    import BackTop from 'components/content/backTop/BackTop'

    import {getHomeMultidata,getHomeGoods} from 'network/home';
    import {debounce} from 'common/unils'
    export default {
        name:"Home",
        components:{
            HomeSwiper,
            RecommendView,
            FeatureView,
            NavBar,
            TabControl,
            GoodsList,
            Scroll,
            BackTop
        },
        data(){
            return{
               banners:[],
               recommends:[],
               goods:{
                   'pop':{page: 0,list:[] },
                   'new':{page: 0,list:[] },
                   'sell':{page: 0,list:[] }
               },
               currentType: 'pop',
               isShowBackTop: false,
               tabOffsetTop: 546,
               isTabFixed: false
            }
        },
        computed:{
            showGoods(){
                return this.goods[this.currentType].list
            }
        },
        created(){
            //请求多个数据
            this.getHomeMultidata()
            //请求商品数据
            this.getHomeGoods('pop')
            this.getHomeGoods('new')
            this.getHomeGoods('sell')    
        },
        mounted(){
           const refresh = debounce(this.$refs.scroll.refresh,200)
            //监听item中图片加载完成
            this.$bus.$on('itemImageLoad',()=>{
                refresh()
            })

        },
        methods:{
        //事件监听相关方法
            tabClick(index){
                switch(index){
                    case 0:
                        this.currentType = 'pop'
                        break
                    case 1:
                        this.currentType = 'new'
                        break
                    case 2:
                        this.currentType = 'sell'
                        break
                }
                this.$refs.tabControl1.currentIndex = index;
                this.$refs.tabControl2.currentIndex = index;
            },
            backClick(){
                this.$refs.scroll.scrollTo(0,0,500)
            },
            contentScroll(position){
                //判断BackTop是否显示
                this.isShowBackTop = (-position.y) > 1000
                //决定tabControl是否吸顶
               
                this.isTabFixed = (-position.y) > this.tabOffsetTop
            },
            loadMore(){
                this.getHomeGoods(this.currentType)
            },
            swiperImageLoad(){
               this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
            },
        //网络请求相关方法
            getHomeMultidata(){
                getHomeMultidata().then(res=>{
                    this.banners = res.data.banner.list;
                    this.recommends = res.data.recommend.list
                })
            },
            getHomeGoods(type){
                const page = this.goods[type].page + 1
                getHomeGoods(type,page).then(res=>{
                    this.goods[type].list.push(...res.data.list)
                    this.goods[type].page = page
                    //完成上拉加载更多
                    this.$refs.scroll.finishPullUp()
                })
            }
        }
    }
</script>

<style scoped>
    #home{
        height: 100vh;
        position: relative;
    }
    .home-nav{
        background-color: var(--color-tint);
        color:#fff;
    }
    
    .content{
        overflow: hidden;
        position: absolute;
        top: 44px;
        bottom: 49px;
        left: 0;
        right: 0;
    }
    .tab-control{
        position: relative;
        z-index: 9
    }
</style>