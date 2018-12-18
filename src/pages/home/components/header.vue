<template>
  <div class="header">
    <a class="header-left iconfont back-icon">&#xe624;</a>
    <div class="header-input">
      <a>
        <i class="icon-search iconfont">&#xe632;</i>
        <span class="single-line">输入城市/景点/游玩主题</span>
      </a>
    </div>
    <router-link to='/city'>
    <div class="header-right">
      <a href="" class="mp-linkarea">
        <span class="mp-nav-city">{{this.$store.state.city}}
        <i class="arrow-icon iconfont ">&#xe64a;</i>
        </span>
      </a>
    </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'HomeHeader',
  }
</script>

<style lang="stylus">
@import '../../../assets/styles/varibles.styl'
.header
  position:relative;
  display :-webkit-box;
  width:100%;
  height:.88rem;
  background :$bgColor;
  text-align:center;
  color #fff;
.header-left
  display:inline-block;
  width:.4rem;
  line-height:.68rem;
  padding:0.1rem;
  color:#fff;
  font-size:.36rem;
  text-align:left;
  .back-icon
   text-align : center;
   font-size:.4rem;
.header-input
  box-sizing:border-box;
  flex :1;
  height :.6rem;
  position:relative;
  margin:.14rem .09rem;
  background :#fff;
  line-height:.6rem;
  border-radius:.06rem;
  color:#ccc;
.icon-search
 position:absolute;
 left:.2rem;
 color:#e4e7ea;
.single-line
 display:inline-block;
 overflow:hidden;
 position:absolute;
 left:.3rem;
 top:0;
 color:#e4e7ea;
 width:3.6rem;
 white-space:nowrap;
 text-overflow:ellipsis; 
.header-right
 height:100%;
 .mp-linkarea
  display:inline-block;
  line-height:.88rem;
  margin-left:.04rem;
  color:#fff;
  .mp-nav-city
   padding:0.22rem;
 .arrow-icon
  margin-left:0rem;
  font-size:.24rem;
</style>