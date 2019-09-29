<template>
      <div class="wrpa">
          <header>
               <img src="../../assets/img/avatar.jpg" alt="">
          </header>
          
          <div class="tit">
               爱时尚精品屋
          </div>
          <div class="mess">
              公告：欢迎光临流行瀑布爆款库存有限请尽早下单，谢谢
          </div>
          <nav> 
              <div class="tab">
                  <li v-for='(item,index) in navList' 
                  :key="index" 
                  :class="{active: index==curIndex }" @click="dian(index)">{{item}}</li>
              </div>
              <div class="box" v-for="(item,index) in list" :key='index'>
                  <div class="left">
                      <img :src="item.pic" alt="">
                  </div>
                   <div class="right" >
                       <p>{{item.name}}</p>
                       <p>已售{{item.buyCount}}</p>
                        <p>单价:{{item.price}}</p> 
                    
                  </div>

                     <div class="btn" @click="addToCart(item)">
                         加入购物车
                  </div>
                 
              </div>
               
          </nav>
         <router-link to='/cart'>
             <div class="ico">
                    <img src="../../assets//img/cart.png" alt="">
                    <span>{{total}}</span>
            </div>
         </router-link>
          
         
      </div>
</template>
<script>
import axios from 'axios'
export default {
    data () {
        return {
            curIndex:0,
            goodInCart:[],//保存加入购物车的商品
            navList:['价格升序^','价格降序','销量升序^','销量降序'],
            list:[]
        }
    },
    computed: {
         total(){
             return this.goodInCart.reduce((t,i)=>{
                 return t+i.count
             },0)
         }  
    },
    methods: {
        dian(index){
           this.curIndex=index   
           if(index==0){
               this.list.sort((gooA,gooB)=>{
                   return gooA.price - gooB.price;
               })
           }else if(index==1){
               this.list.sort((gooA,gooB)=>{
                    return gooB.price-gooA.price
               })
              
           }else if(index==2){
                this.list.sort((gooA,gooB)=>{
                    return gooA.buyCount-gooB.buyCount
               })
           }else{
                this.list.sort((gooA,gooB)=>{
                    return gooB.buyCount-gooA.buyCount
               })
           }     
        },
        addToCart(good){
            let goodInCart=localStorage.getItem('goodInCart')?JSON.parse(localStorage.getItem('goodInCart')):[]
            var findGood=goodInCart.find((item,index)=>{
                return good.id==item.id
            })
            console.log(findGood);
            if(!findGood){
                good.count=1;
                goodInCart.push(good)
            }else{
                findGood.count++
            }
            this.goodInCart=goodInCart;
            console.log(goodInCart)
            localStorage.setItem('goodInCart',JSON.stringify(goodInCart));
        }
    },
    mounted () {
        {
            axios.get("https://yantianfeng.com/api/goodList").then(res=>{
                 console.log(res.data);
                 this.list=res.data.goodList
                 console.log(this.list);
                 
            })
            this.goodInCart=localStorage.getItem('goodInCart')?JSON.parse(localStorage.getItem('goodInCart')):[]
        }
    }
}
</script>

<style lang="scss" scoped>
.wrpa{
     width: 100%;
     height: 100%;
     display: flex;
     flex-direction: column;
     overflow: hidden;
}
header{
    height: 200px;
    background-size: cover;
    background-position: center;
    background-image:url('../../assets//img/banner-2.png') ;
    position: relative;
    img{
        width: 80px;
        height: 80px;
        border-radius: 50%;
        position: absolute;
        bottom: -40px;
        left: 50%;
        margin-left: -40px; 
    }
}
.tit{
    font-size: 20px;
    font-weight: bold;
   margin-top: 50px;
   text-align: center;
}
.mess{
     font-size: 12px;
   margin-top: 10px;
   text-align: center;
}
.tab{
    display: flex;
    margin-top: 30px;
    justify-content: space-around;
    border-bottom: 1px solid #cccccc;
    font-size: 18px;
}
.box{
    display: flex;
    margin: 0 20px;
    margin-top: 10px;
     position: relative;
}
.left{
    width: 30%;
    img{
        width: 100%;
    }
}
.right{
    width: 70%;
    margin-left: 10px;
    line-height: 30px;
   
}
.right :first-child{
     white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
   
}
.active{
    color: orchid
}
nav{
    flex: 1;
    overflow-y:scroll;
}
 .btn{
    position: absolute;
    right: 0;
    bottom: 10px;
    background-color: red;
    color: white;
    text-align: center;
    padding: 5px 15px;
    font-size: 14px;
  }
  .ico{
       position: fixed;
      background: palegoldenrod;
      width: 60px;
      height: 60px;
      right: 10px;
      bottom: 30px;
      border-radius: 50%;
      text-align: center;
      img{
         margin-top: 10px;
         width: 40px;
         height: 40;
      }
  }
  .ico span{
       position: fixed;
    display: inline-block;
     background: red;
      width: 20px;
      height: 20px;
      color: #ffffff;
      right: 11px;
      bottom: 72px;
      border-radius: 50%;
      text-align: center;
  }
</style>