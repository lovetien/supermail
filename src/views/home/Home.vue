<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
        <home-swiper :banners="banners" />
        <recommend-view :recommends="recommends" />
        <feature-view />
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
            <li>6</li>
            <li>7</li>
            <li>8</li>
            <li>9</li>
            <li>10</li>
            <li>11</li>
            <li>12</li>
            <li>13</li>
            <li>14</li>
            <li>15</li>
            <li>16</li>
            <li>17</li>
            <li>18</li>
            <li>19</li>
            <li>20</li>
        </ul>
    </div>
</template>

<script>
    import NavBar from 'components/common/navbar/NavBar';
    import HomeSwiper from './childComps/HomeSwiper';
    import RecommendView from './childComps/RecommendView';
    import FeatureView from './childComps/FeatureView'
    import {getHomeMultidata} from 'network/home';
    export default {
        name:"Home",
        components:{
            NavBar,
            HomeSwiper,
            RecommendView,
            FeatureView
        },
        data(){
            return{
               banners:[],
               recommends:[]
            }
        },
        created(){
            getHomeMultidata().then(res=>{
                this.banners = res.data.banner.list;
                this.recommends = res.data.recommend.list
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
        color:#fff;
        position: fixed;
        left: 0;
        right: 0;
        top: 0;
        z-index: 9;
    }
</style>