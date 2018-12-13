<template>
<div>
    <div class="title">
        周末去哪儿
    </div>
    <ul>
        <li class="item border-bottom" v-for="item of list " :key="item.id" >
            <div class="item-img-wrapper">
            <img class="item-img" :src="item.imgUrl" alt="">
            </div>
                <div class="item-info">
                <p class="item-title">{{item.title}}</p>
                <p class="item-desc">{{item.desc}}</p>
            </div>
        </li>
    </ul>
</div>
</template>


<script>
export default{
    name:'HomeRecommend',
    props:{
        list:Array
    }
}
</script>

<style lang="stylus" scoped>
 .title
  line-height:.8rem;
  background:#eee;
  text-align:.2rem;
  .item-img-wrapper
    overflow:hidden;
    height:0;
    padding-bottom:37.09%;
 .item-img
  width:100%;
 .item-info
  padding:.1rem;
  .item-title
   line-height:.54rem;
   font-size:.32rem;
   overflow:hidden;
   white-space:nowrap;
   text-overflow:ellipsis;
  .item-desc
   overflow:hidden;
   padding-right:1.4rem;
   font-size:.24rem;
   line-height:.42rem;
   color:#616161;
   overflow:hidden;
   white-space:nowrap;
   text-overflow:ellipsis;   
</style>