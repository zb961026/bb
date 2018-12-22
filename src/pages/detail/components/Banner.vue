<template>
    <div>
    <div class="banner" @click="handleBannerClick">
        <img class="banner-img" :src="bannerImg"/>
        <div class="banner-info">
            <div class="banner-title">{{this.sightName}}</div>
        </div>
    </div>
    <common-gallary :imgs="bannerImgs" v-show="showGrally" @close="handleGallaryClose"></common-gallary>
    </div>
</template>

<script>
import CommonGallary from 'common/gallary/Gallary'
import FadeAnimation from 'common/fade/FadeAnimation'
    export default{
        name:'DetailBanner',
        props:{
            sightName:String,
            bannerImg:String,
            bannerImgs:Array,
        },
        data(){
            return{
                showGrally:false,
            }
        },
        methods: {
            handleBannerClick(){
                this.showGrally=true
            },
            handleGallaryClose(){
                this.showGrally=false
            }
        },
        components:{
            CommonGallary,FadeAnimation
        }
    }
</script>

<style lang="stylus" scoped>
    .banner
      position:relative;
      overflow:hidden;
      height:0;
      padding-bottom:55%
      .banner-img
         width:100%;
      .banner-info
        display:flex;
        position:absolute;
        left:0;
        right:0;
        bottom:0;
        line-height:.6rem;
        color:#fff;
        background-image:linear-gradient(top,rgba(0,0,0,0),rgba(0,0,0,0.8))
        .banner-title
          felex:1;
          font-size:.36rem;
          padding:0.2rem;
</style>