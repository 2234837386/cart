<template>
      <div class="wrap_cart">
          <div class="title">
              购物车
          </div>
          <div class="list">
              <div class="box" v-for="(item,index) in goodInCart" :key='index'>
                  <div class="left">
                      <img :src="item.pic" alt="">
                  </div>
                   <div class="right" >
                       <p>{{item.name}}</p>
                       <p>已售{{item.buyCount}}</p>
                        <p>单价:{{item.price}}</p> 
                    
                  </div>

                 
              </div>
          </div>
            
      </div>
</template>

<script>
export default {
   data () {
       return {
           goodInCart:[]
       }
   },
   computed: {
       total(){
           return this.goodInCart.reduce((t,good)=>{
               return t+good.count
           },0)
       }
   },
   //请求数据
   mounted:function()
  {
    this.goodInCart = localStorage.getItem('goodInCart')? JSON.parse(localStorage.getItem('goodInCart')):[];
  }

}
</script>

<style lang='scss' scoped>
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
</style>