travel去哪儿网项目开发流程

0-1：项目初始化，装包


一 主页:home.vue

第一页面：


0-2：在组件home中声明一个模板{
<template>
<div>
  <home-header :city="city"></home-header>//使用homeheader组件，通过属性把下面data的city传递给home-header组件
  <home-swiper :list="swiperList"></home-swiper>//使用轮播组件，通过属性把下面data的swiper传递给home-swiper
  <home-icons :list="iconList"></home-icons>//使用图标组件,通过属性把下面data的iconlist传递给home-icons组件
  <home-recommend :list="recommendList"></home-recommend>//使用推荐游玩组件，通过属性把下面data的recommendlist传递给home-recommend组件
  <home-weekend :list="weekendList"></home-weekend>//使用周末游玩组件，通过属性把下面data的weekendlist传递给home-weekend组件
</div>
</template>

<script>
import HomeHeader from './components/Header'//引入header组件
import HomeSwiper from './components/Swiper'//引入swiper轮播组件
import HomeIcons from './components/Icons'//引入图标组件
import HomeRecommend from './components/Recommend'//引入推荐游玩组件
import HomeWeekend from './components/Weekend'//引入周末游玩组件
import axios from 'axios'//引入axios数据才能发ajax请求
export default {
  name: 'Home',
  components: {//声明局部组件
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data(){//存储页面的各种数据
    return{//返回为空数值类型
      city:'',//设置为空
      swiperList: [],//空数组
      iconList: [],//空数组
      recommendList:[],//空数组
      weekendList:[],//空数组
    }
  },
  methods:{
    getHomeInfo(){//获取ajax数据的方法
      axios.get('/api/index.json')//返回的是一个promise对象，后面使用then
      .then(this.getHomeInfoSucc)//获取成功执行的函数
    },
    getHomeInfoSucc(res){//res是响应的意思
      res=res.data//res为获取json数据的内容
      if(res.ret&&res.data){//如果后端正确的返回了结果，而且res有data这个内容项
        const data=res.data//设置数据为响应获得的数据
        this.city=data.city//数据中的城市，传递给了子组件header，city如果发生变化，子组件就会跟着变
        this.swiperList=data.swiperList;//数据中的轮播列表
        this.iconList=data.iconList;//数据中的图标列表
        this.recommendList=data.recommendList;//数据中的推荐游玩列表
        this.weekendList=data.weekendList;//数据中的周末游玩列表
      }
    }
  },
  mounted(){
    this.getHomeInfo()//页面挂载好执行这个函数ajax获取数据
}
  }
  }
  </script>
</temple>

2.创建header组件

<template>
  <div class="header">
    <a class="header-left iconfont back-icon">&#xe624;</a>//字体图标
    <div class="header-input">
      <a>
        <i class="icon-search iconfont">&#xe632;</i>//字体图标
        <span class="single-line">输入城市/景点/游玩主题</span>
      </a>
    </div>
    <router-link to='/city'>//点击区域跳转到/city
    <div class="header-right">
      <a href="" class="mp-linkarea">
        <span class="mp-nav-city">{{this.city}}
        <i class="arrow-icon iconfont ">&#xe64a;</i>//字体图标
        </span>
      </a>
    </div>
    </router-link>//跳转路由
  </div>
</template>

<script>
export default {
  name: 'HomeHeader',
  props:{//声明一个props接收父组件home.vue传递过来的值
    city:String//接收city的数据类型，必须为string类型
  }
  }
</script>

<style lang="stylus" scoped>
@import '../../../assets/styles/varibles.styl'
1rem=html font-size=50px
.header
  position:relative;//相对定位
  display :-webkit-box;
  width:100%;
  height:.88rem;
  background :$bgColor;
  text-align:center;
  color #fff;
.header-left
  display:inline-block;//行内块
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
  box-sizing:border-box;//内边距和边框不再会增加它的宽度
  flex :1;//flex弹性盒子
  height :.6rem;
  position:relative;
  margin:.14rem .09rem;//上下外边距，左右外边距
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
 white-space:nowrap;//规定段落中文本不换行
 text-overflow:ellipsis; //文本超出用省略号显示
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

3.创建swiper组件

<template>
    <div class="wrapper">//在图片外加一个div，不然网速太慢会有抖动感
    <swiper :options="swiperOption" v-if="showSwiper">//swiper轮播图插件，swipeription是绑定遍历，所以要写一个data，返回一些数据，用v-if判断length是不是完整的数据列表，当list。length是空时，是false，所以轮播不会被创建，这样能保证轮播图默认显示第一张图片
    <swiper-slide v-for="item of list" :key="item.id">//用v-for循环列表，用id来获取循环
        <img  class="swiper-img" :src="item.imgUrl">循环，来绑定获取图片链接
    </swiper-slide>
    <div class="swiper-pagination" slot="pagination"></div>//显示分页
  </swiper>
  </div>
</template>

<script>
    export default{
        name:'HomeSwiper',//声明局部组件
        props:{
            list:Array//接收父组件传递过来的，必须是一个数组类型
        },
        data(){
            return{
                swiperOption:{
                    pagination:'.swiper-pagination' ,//填写一个配置项，传入显示分页的class
                    loop:true//新加配置项，让插件支持循环轮播         
                },
            }
        },
        computed:{//计算属性
            showSwiper(){
                return this.list.length//计算列表的长度是不是正确
            }

        }
    }
</script>

<style lang="stylus" scoped>
  .wrapper>>>.swiper-pagination-bullet-active//击穿div，设置div的css样式
   background-color :#fff;
  .wrapper
   width :100%; 
   height :0;//如果高度写成31.25，相对撑开的是父级的高度
   overflow: hidden
   padding-bottom:32.25%;//宽度相对高度撑开31.25%
  .swiper-img
    width : 100%;
</style>

4.创建icons图标组件

<template>
<div class="mp-category-container">
    <div class="mpw-swipe-wrap">
        <swiper>
        <swiper-slide v-for="(page, index) of pages" :key="index">//循环输出
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
        name:'HomeIcons',//声明局部组件
        props:{
            list:Array//接收父组件home.vue传递过来的list，list类型必须是数组
        },
        computed:{//计算属性，自带缓存机制
            pages (){//定义pages，是函数返回的
        const pages=[]//定义数组
        this.list.forEach((item,index)=>{//对list每一项进行循环，会接收两个参数，一个是具体的循环项，一个是下标
            const page=Math.floor(index/8)//获取第几个数据该展示在第几页上面
            if(!pages[page]){//如果pages不存在
                pages[page]=[]//空数组
            }
            pages[page].push(item)//把pages空数组添加到里面
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

5.创建推荐游玩组件

<template>
<div>
    <div class="title">
        <img src="https://imgs.qunarzz.com/piao/fusion/1711/89/ebc329f16c55bb02.png" width="20px" height="20px" alt="">
        热销推荐
    </div>
    <ul>
        <li class="item border-bottom" v-for="item of list" :key="item.id" >
                <img class="item-img" :src="item.imgUrl" alt="">
                <div class="item-info">
                <p class="item-title">{{item.title}}</p>
                <p class="item-desc">{{item.desc}}</p>
                <button class="item-button">查看详情</button>
            </div>
        </li>
    </ul>
</div>
</template>


<script>
export default{
    name:'HomeRecommend',
    props:{
        list:Array//接收父组件传来的值，数据类型必须是数组
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

6.创建周末游玩组件

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
        list:Array////接收父组件传来的值，类型必须是数组
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

路由配置：

index.js

import Vue from 'vue'
import Router from 'vue-router'
import Home from '@/pages/home/Home'
import City from '@/pages/city/City'//引入city组件
Vue.use(Router)

export default new Router({
    routes: [{
        path: '/',//路由路径
        name: 'Home',//路由名字
        component: Home//对应组件
    }, {
        path: '/city',//路由路径
        name: 'City',//路由名字
        component: City//对应组件
    }]
})
第二页面

City.vue

<template>
<div>
<city-header></city-header>
<city-search :cities="cities"></city-search>//
<city-list :cities="cities" :hot="hotCities" :letter="letter"></city-list>//下面methods的letter传递过来
<city-alphabet :cities="cities" @change="handleLetterChange"></city-alphabet>//监听一下change事件
</div>
</template>

<script>
import axios from 'axios'
import CityHeader from './components/Header'
import CitySearch from './components/Search'
import CityList from './components/List'
import CityAlphabet from './components/Alphabet'
export default{
    name:'City',
    components:{
        CityHeader,
        CitySearch,
        CityList,
        CityAlphabet
    },
    data(){
        return{
            cities:{},
            hotCities:[],
            letter:'',
        }
    },
    methods: {
        getCityInfo () {
            axios.get('/api/city.json')
               .then(this.handleGetCityInfoSucc)
        },
        handleGetCityInfoSucc (res) {
            res=res.data;
            if(res.ret&&res.data)
            {
                const data=res.data
                this.cities=data.cities
                this.hotCities=data.hotCities
            }
        },
        handleLetterChange (letter) {//会接收到一个letter
            this.letter=letter//当接收到外部传来的letter
        }
    },
    mounted() {
        this.getCityInfo()
    },

}
</script>

<style lang="stylus" scoped>
</style>


1.header.vue

<template>
    <div class="header">城市选择
        <router-link to="/">
        <div class="iconfont header-back">&#xe624;</div>
        </router-link>
    </div>
</template>

<script>
    export default{
        name:'CityHeader'
    }
</script>

<style lang="stylus" scoped>
@import '../../../assets/styles/varibles.styl'
.header
   line-height:.86rem;
   overflow: hidden; 
   text-align:center;
   color:#fff;
   background:$bgColor;
   font-size:.32rem;
   position:relative;
   .header-back
     position:absolute;
     top:0;
     left:0;
     width:.64rem;
     text-align:center;
     font-size:.4rem;
     color:#fff;
</style>

2.search.vue

<template>
<div>
<div class="search">
    <input v-model="keyword" class="search-input" type="text" placeholder="输入城市名或拼音"/>
</div>
<div class="search-content" ref="search" v-show="keyword">//如果keywords有值，就会显示这个div
    <ul>
        <li class="search-item border-bottom" v-for="item of list" :key="item.id">{{item.name}}</li>//循环出有关键词的数据
        <li class="search-item border-bottom" v-show="!list.length">没有找到匹配数据</li>//list长度为0，才显示没有找到匹配数据
    </ul>
</div>
</div>
</template>

<script>
import Bscroll from 'better-scroll'//引入beeter-scroll
    export default{
        name:'CitySearch',
        props:{
            cities:Object//接收到传来的cities
        },
        data(){
            return{
                keyword:'',
                list:[],
                timer:null
            }
        },
        watch:{
            keyword(){
                if(this.timer){
                    clearTimeout(this.timer)
                }
                if(!this.keyword){//如果没有keywords
                    this.list=[]//变成空数组，return
                    return
                }
                this.timer=setTimeout(()=>{
                    const result=[]//空数组
                    for (let i in this.cities){//循环cities的内容
                        this.cities[i].forEach((value)=>{
                            if(value.spell.indexOf(this.keyword)>-1 || value.name.indexOf(this.keyword)>-1)//如果在数据里面能搜索到关键词{
                                result.push(value)//就把这项添加到数组之中
                            }
                        })
                    }
                    this.list=result;//把列表变成包含关键词的result
                },100)
            }
        },
        mounted(){
            this.scroll=new Bscroll(this.$refs.search)//传入search的dom元素
        }
    }
</script>

<style lang="stylus" scoped>
@import '../../../assets/styles/varibles.styl'
  .search
     height:.72rem;
     padding:0 .1rem;
     background:$bgColor;
     .search-input
      box-sizing:border-box;
      height:.62rem;
      line-height:.62rem;
      padding:0 .1rem;
      width:100%;
      text-align:center;
      border-radius:.06rem;
      color:#666;
   .search-content
      z-index:1;
      overflow:hidden;
      position:absolute;
      top:1.58rem;
      left:0;
      right:0;
      bottom:0;
      background:#eee;
      .search-item
       line-height:.62rem;
       padding-left:.2rem;
       color:#666;
       background:#fff;

</style>

3.list.vue

<template>
    <div class="list" ref="wrapper">//ref能帮助获取dom
        <div>
        <div class="area">
            <div class="title border-topbottom">当前城市</div>
            <div class="button-list">
                <div class="button-wrapper">
                    <div class="button">北京</div>
                </div>
            </div>
        </div>
        <div class="area">
            <div class="title border-topbottom">热门城市</div>
            <div class="button-list">
                <div class="button-wrapper" v-for="item of hot" :key="item.id">
                    <div class="button">
                        {{item.name}}
                    </div>
                </div>
        </div>
        <div class="area" v-for="(item, key) of cities" :key="key" :ref="key">
            <div class="title border-topbottom">{{key}}</div>
            <div class="item-list">
                <div class="item border-bottom" v-for="innerItem of item" :key="innerItem.id" >{{innerItem.name}}</div>
            </div>
        </div>
        </div>
    </div>
    </div>
    
</template>

<script>
    import Bsroll from 'better-scroll'//引入better-scroll这个包，页面就能使用滚动插件了
    export default{
        name:'CityList',
        props:{
            hot:Array,
            cities:Object,
            letter:String//接收到传来的letter
        },
        mounted() {
            this.scroll=new Bsroll(this.$refs.wrapper)//在页面的dom挂载完毕后执行，然后就能滚动了
        },
        watch:{//监听器
            letter(){//监听letter变化
                if(this.letter){//如果letter不为空
                    const element=this.$refs[this.letter][0]//可以根据refs来获取到字母对应的一个区域
                    this.scroll.scrollToElement(element)//自动滚动到指定元素上，从而实现字母表点击跳越了
                               }
            }
        }
    }
</script>

<style lang="stylus" scoped>
@import '../../../assets/styles/varibles.styl'
  .border-topbottom
    &:before
      border-color:#ccc;
    &:after
      border-color:#ccc;
  .border-bottom
    &:before
      border-color:#ccc;
 .list
  overflow:hidden;
  position:absolute;
  top:1.58rem;
  left:0;
  right:0;
  bottom:0; 
  .title
   line-height:.44rem;
   background:#eee;
   padding-left:.2rem;
   color:#666;
   font-size:.26rem;
  .button-list
    overflow:hidden;
    padding:.1rem .6rem .1rem .1rem;
   .button-wrapper
     float:left;
     width:33.33%;
    .button
      margin:.1rem .1rem;
      padding:.1rem 0;
      border-radius:.06rem;
      text-align:center;
      border:.02rem solid #ccc;
   .item-list
      .item
        line-height:.76rem;
        padding-left:.2rem;
</style>

4.alphabet.vue

<template>
    <div class="list">
        <li class="item" v-for="item of letters" :key="item" :ref="item" @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd" @click="handleLetterClick">{{item}}</li>//绑定三个touch事件，分别是start，move，end.
    </div>
</template>

<script>
  export default{
      name:'CityAlphabet',
      props:{
          cities:Object
      },
      computed:{
          letters(){//存储数组
              const letters=[]//空数组来存储字母列表
              for(let i in this.cities){
                  letters.push(i)
              }
              return letters
        }
      },
      data() {
          return{
              touchStatus:false,//写一个标识位来确定是否触发了，默认是false
              startY:0,
              timer:null
          }
      },
      updated(){//当页面的数据被更新时，同时页面完成了自己的渲染之后，updated就会被执行。
          this.startY=this.$refs['A'][0].offsetTop//开始的坐标位置
      },
      methods:{
          handleLetterClick(e){//点击后会接收到e的事件对象
              this.$emit('change',e.target.innerText)//向外触发事件change，内容为e.target.innerText
          },
          handleTouchStart (){
              this.touchStatus=true;//手指触摸就会变成true
          },
          handleTouchMove (e){
                if(this.touchStatus){//触摸事件 
                    if(this.timer){//如果存在定时器
                        clearTimeout(this.timer)//去除
                    }
                    this.timer=setTimeout(()=>{//函数截流：延迟执行，假设在时间内操作，会清除上次的操作，可以大大节约执行频率，从而提高网页的性能
                        const touchY=e.touches[0].clientY-79;//当前手指移动距离顶部的高度
                  const index=Math.floor((touchY-this.startY)/20)//手指滑动到的位置对应的字母下标是多少
                  if(index>= 0&&index<this.letters.length){//当字母下标大于等于0，且小于字母列表的总长度
                      this.$emit('change',this.letters[index])//向外触发，改变字母下标，所以右侧字母列表一拖动，左边字母的地方区域就会对应地滚动。点击字母列表也是一样的效果
                  }
                },16)
              }

          },
          handleTouchEnd(){
              this.touchStatus=false;//触摸结束后变成默认的false

          }
      }
  }  
</script>

<style lang="stylus" scoped>
@import '../../../assets/styles/varibles.styl'
 .list
    display:flex;
    flex-direction:column;
    justify-content:center;
    position:absolute;
    top:1.58rem;
    right:0;
    bottom:0;
    width:.4rem;
    .item
     line-height:.4rem;
     text-align:center;
     color:$bgColor;
     list-style:none;

</style>