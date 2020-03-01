<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs class="order-tabs">
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border>
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="60"></el-table-column>
              <el-table-column prop="price" label="金额" width="60"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-tab-pane>
          <div class="total">
            <span>总价：{{totalMoney}}</span>
            <span>数量：{{totalCount}}</span>
          </div>
          <div class="jiesuan" >
            <el-button type="warning" @click="delAll">清空</el-button>
            <el-button type="success" @click="checkOut">结账</el-button>
          </div>
          <el-tab-pane label="挂单"></el-tab-pane>
          <el-tab-pane label="外卖"></el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="15">
        <div class="o-goods">
          <div class="title">常用商品</div>
          <div class="o-goods-list">
            <ul>
              <li class="o-goods-list" v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type clearFloat">
          <el-tabs class="marginLeft">
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="goods in hanbao" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img src="good.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li v-for="goods in xiaochi" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img src="good.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="goods in yinpin" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img src="good.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li v-for="goods in taocan" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img src="good.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      totalMoney: 0,
      totalCount: 0,
      tableData: [],
      oftenGoods: [
        {
          goodsId: 1,
          goodsName: "香辣鸡腿堡",
          price: 18
        },
        {
          goodsId: 2,
          goodsName: "田园鸡腿堡",
          price: 15
        },
        {
          goodsId: 3,
          goodsName: "和风汉堡",
          price: 15
        },
        {
          goodsId: 4,
          goodsName: "快乐全家桶",
          price: 80
        },
        {
          goodsId: 5,
          goodsName: "脆皮炸鸡腿",
          price: 10
        },
        {
          goodsId: 6,
          goodsName: "魔法鸡块",
          price: 20
        },
        {
          goodsId: 7,
          goodsName: "可乐大杯",
          price: 10
        },
        {
          goodsId: 8,
          goodsName: "雪顶咖啡",
          price: 18
        },
        {
          goodsId: 9,
          goodsName: "大块鸡米花",
          price: 15
        },
        {
          goodsId: 20,
          goodsName: "香脆鸡柳",
          price: 17
        }
      ],
      hanbao: [
        {
          goodsId: 1,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
          goodsName: "香辣鸡腿堡",
          price: 18
        },
        {
          goodsId: 2,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          goodsName: "田园鸡腿堡",
          price: 15
        },
        {
          goodsId: 3,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
          goodsName: "和风汉堡",
          price: 15
        }
      ],
      xiaochi: [
        {
          goodsId: 9,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          goodsName: "大块鸡米花",
          price: 15
        },
        {
          goodsId: 20,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          goodsName: "香脆鸡柳",
          price: 17
        },
        {
          goodsId: 5,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          goodsName: "脆皮炸鸡腿",
          price: 10
        },
        {
          goodsId: 6,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
          goodsName: "魔法鸡块",
          price: 20
        }
      ],
      yinpin: [
        {
          goodsId: 7,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
          goodsName: "可乐大杯",
          price: 10
        },
        {
          goodsId: 8,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          goodsName: "雪顶咖啡",
          price: 18
        }
      ],
      taocan: [
        {
          goodsId: 4,
          goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          goodsName: "快乐全家桶",
          price: 80
        }
      ]
    };
  },
  created: function() {
    axios
      .get()
      .then(reponse => {
        console.log(reponse);
      })
      .catch(error => {
        console.log(error);
      });
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;
      //商品是否在订单列表中
      let isHave = false;

      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //根据判断的值编写业务逻辑
      if (isHave) {
        //改变列表中商品数量        ;
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.total();
    },
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.total();
    },
    delAll(){
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    total() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },
    checkOut(){
      if(this.totalCount !=0){
        this.totalCount = 0;
        this.totalMoney = 0;
        this.tableData = [];
        this.$message.success('结账成功');
      }else{
        this.$message.error('无法空结')
      }
    }
  }
};
</script>

<style scoped>
li {
  list-style: none;
}
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.order-tabs {
  margin-left: 10px;
}
.jiesuan {
  margin-top: 15px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: leftl;
}
.o-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #d3dce6;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.clearFloat {
  clear: both;
}
.marginLeft {
  margin-left: 10px;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
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
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
