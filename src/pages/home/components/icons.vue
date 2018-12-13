<template>
<div class="mp-category-container">
    <div class="mpw-swipe-wrap">
        <swiper>
        <swiper-slide v-for="(page, index) of pages" :key="index">
        <div class="mpw-swipe-item">
            <div class="mp-category-item" v-for="item of page" :key="item.id">
                <a href="">
                    <div class="mp-category-img-container">
                        <img :src='item.imgUrl' width="55px" height="55px">
                    </div>
                    <div class="keywords">{{item.keywords}}</div>
                </a>
            </div>
        </div>
        </swiper-slide>
        </swiper>
    </div>
    </div>
</template>

<script>
    export default{
        name:'HomeIcons',
        props:{
            list:Array
        },
        computed:{
            pages (){
        const pages=[]
        this.list.forEach((item,index)=>{
            const page=Math.floor(index/8)
            if(!pages[page]){
                pages[page]=[]
            }
            pages[page].push(item)
        })
        return pages
            }
        }
    }
</script>

<style lang="stylus" scoped>
  .mp-category-container
   overflow:hidden;
   height:3.7rem;
   padding-top:.1rem;
   background-color:#fff;
   .mpw-swipe-item
    width:375px;
    height:3.7rem;
    left:0;
    border-bottom:1px solid red;
  .mpw-swipe-wrap
   width:750px;
   position:relative;
  .mp-category-item
   float:left;
   width:25%;
   height:1.5rem;
   padding-top:.1rem;
   text-align:center;
  .mp-category-item>>>.keywords
   margin-top:.1rem;
   color:#212121;
   font-size:.28rem;
   overflow:hidden;
   white-space:nowrap;
   text-overflow:ellipsis;
   .mp-category-img-container
    display:inline-block;
    width:1.1rem;
    height:1.1rem;
</style>