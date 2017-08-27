<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="8">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="dataGoodsList"  border style="width:100%;">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="goodsCount" label="量"></el-table-column>
                <el-table-column prop="goodsPrice" label="单价"> </el-table-column>
                <el-table-column  label="操作">
                    <template scope="scope">
                      <el-button type="text" size="small" @click="deleteGoods(scope.row)">删除</el-button>
                      <el-button type="text" size="small" @click="addGoods(scope.row)">增加</el-button>
                    </template>
                </el-table-column>
              </el-table>
              <div class="totalShow">
                <span>数量：{{ totalCount }}</span>
                <span>金额：{{ totalPrice }}元</span>
              </div>
              <!--操作按钮-->
              <div class="btn-operate">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="deleteAll()">删除</el-button>
                <el-button type="success" @click="pay">结账</el-button>
              </div>
            </el-tab-pane>

            <el-tab-pane label="挂单">

            </el-tab-pane>

            <el-tab-pane label="外卖">

            </el-tab-pane>
          </el-tabs>

        </el-col>
        <el-col :span="16">
          <!--常用食品-->
          <div class="often-goods">
            <div class="often-goods">
              <div class="title">常用商品</div>
                <div class="often-goods-list">
                  <ul>
                    <li v-for="goods in oftenGoodsList" @click = "addGoods(goods)">
                      <span>{{ goods.goodsName }}</span>
                      <span class="o-price">{{ goods.price }}</span>
                    </li>
                  </ul>
                </div>
            </div>
          </div>
          <!--分类列表-->
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="item in typeGoods[0]" @click="addGoods(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName"> {{ item.goodsName }} </span>
                    <span class="foodPrice">￥ {{ item.price }} 元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class='cookList'>
                  <li v-for="item in typeGoods[1]" @click="addGoods(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName"> {{ item.goodsName }} </span>
                    <span class="foodPrice">￥ {{ item.price }} 元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class='cookList'>
                  <li v-for="item in typeGoods[2]" @click="addGoods(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName"> {{ item.goodsName }} </span>
                    <span class="foodPrice">￥ {{ item.price }} 元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class='cookList'>
                  <li v-for="item in typeGoods[3]" @click="addGoods(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName"> {{ item.goodsName }} </span>
                    <span class="foodPrice">￥ {{ item.price }} 元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
import ax from 'axios'
export default {
  created() {
    ax.get('/api/oftenGoods')
      .then((response)=>{
        this.oftenGoodsList = response.data;
      })
      .catch(error=>{
        console.log(error);
        alert('网络错误，不能获取数据！');
      });
    ax.get('api/typeGoods')
      .then((response)=>{
        this.typeGoods = response.data;
      })
      .catch(error=>{
        console.log(error);
        alert('网络错误，不能获取数据！');
      });
  },
  name:'Index',
  data() {
    return {
      oftenGoodsList:[],
      typeGoods:[],
      dataGoodsList: []
    }
  },
  computed:{
    totalCount() {
      var count = 0;
      for(var item of this.dataGoodsList){
        count += item.goodsCount;
      }
      return count;
    },
    totalPrice() {
      var price = 0;
      for(var item of this.dataGoodsList){
        if(item.goodsPrice > 0){
          price += item.goodsPrice * item.goodsCount;
        }
      }
      return price;
    }
  },
  methods:{
    addGoods(goods){
      //如果已经存在,则只增加数量
      //否则新添加一个商品信息
      let isHave = false;
      for(var item of this.dataGoodsList){
        if( item.goodsId == goods.goodsId ){
          isHave = true;
        }
      }
      if(isHave){
        var findGoods = this.dataGoodsList.filter( (o)=> { return o.goodsId == goods.goodsId});
        //console.log(findGoods);
        findGoods[0].goodsCount ++ ;
      }else{
        var newGoods = {
          goodsId : goods.goodsId,
          goodsName: goods.goodsName,
          goodsPrice: goods.price,
          goodsCount : 1
        }
        this.dataGoodsList.push(newGoods);
      }
    },
    deleteAll() {
      this.dataGoodsList = [];
    },
    deleteGoods(goods) {
      let filterGoodsList = this.dataGoodsList.filter( o => o.goodsId != goods.goodsId);
      this.dataGoodsList = filterGoodsList;
    },
    //结账
    pay() {
      if(this.totalPrice > 0 ){
        this.dataGoodsList = [];
        this.$message({message:'付款成功！',type:'success',showClose:true,duration:1500});
      }else{
        this.$message({message:'请先选择商品！',type:'warning',showClose:true,duration:1500});
      }

    }
  }
}
</script>
<style scoped>
  .pos{
    height: 100%;
  }
  .el-table-column{
    text-align: center;
  }

  .btn-operate{
    margin: 10px;
  }

  .title{
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color:#F9FAFC;
    padding:10px;
  }
  .often-goods-list ul li {
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color:#FFFFFF;
    cursor: pointer;
  }
  .o-price {
    color:#58B7FF;
  }
/*分类列表*/
  .goods-type {
    clear:both;
  }
  .cookList li {
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color: #FFFFFF;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
  }
  .cookList li span {
    display: block;
    float: left;
  }
  .foodImg {
    width: 38%;
  }
  .foodName {
    font-size: 16px;
    padding-left: 10px;
    color: brown;
  }
  .foodPrice {
    font-size: 14px;
    padding-left: 10px;
    padding-top: 10px;
  }
  .totalShow {
    margin: 5px auto;
    width: 100%;
    height: 20px;
    line-height: 20px;
    text-align: center;
  }
  .totalShow span {
    width: 30%;
    margin-left: 5px;
  }
</style>
