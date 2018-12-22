<template>
<div>
    <div class="title">
        <img src="https://imgs.qunarzz.com/piao/fusion/1711/89/ebc329f16c55bb02.png" width="20px" height="20px" alt="">
        热销推荐
    </div>
    <ul>
        <router-link tag="li" class="item border-bottom" v-for="item of list" :key="item.id" :to="'/detail/'+item.id" >
                <img class="item-img" :src="item.imgUrl" alt="">
                <div class="item-info">
                <p class="item-title">{{item.title}}</p>
                <p class="item-desc">{{item.desc}}</p>
                <button class="item-button">查看详情</button>
            </div>
        </router-link>
    </ul>
</div>
</template>


<script>
export default{
    name:'HomeRecommend',
    props:{
        list:Array
    },
}
</script>

<style lang="stylus" scoped>
 .title
  margin-top:.2rem;
  line-height:.8rem;
  background:#fff;
  text-align:.2rem;
  padding-left:10px;
  padding-bottom:15px;
 .item
    overflow:hidden;
    display:flex;
    height:1.9rem;
    padding-left:10px;
    .item-img
        width:1.7rem;
        height:1.7rem;
        padding:.1rem;
    .item-info
        flex:1;
        padding:.1rem;
        .item-title
           line-height:.54rem;
           font-size:.32rem;
           overflow:hidden;
           white-space:nowrap;
           text-overflow:ellipsis;
        .item-desc
            line-height:.4rem;
            color:#eee;
            overflow:hidden;
            white-space:nowrap;
            text-overflow:ellipsis;
        .item-button
            width:1.8rem;
            line-height:.15rem;
            margin-top:.2rem;
            background:#ff9300;
            padding:0.1rem;
            border-radius:.06rem;
            color:#fff;     
</style>