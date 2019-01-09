<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order-list">
        <el-tabs class="tab">
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="sumDiv"><small>数量:</small>{{totalCount}} &nbsp;&nbsp;<small>金额:</small>￥{{totalMoney}}元</div>
            <div class="order-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="checkOut">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)"><span>{{goods.goodsName}}</span><span class="o-price">￥{{goods.price}}</span></li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs class="tab">
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                  <li v-for="goods in typeGoods[0]" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%" height="60px"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小吃">
              <ul class='cookList'>
                <li v-for="goods in typeGoods[1]" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%" height="60px"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                <li v-for="goods in typeGoods[2]" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%" height="60px"></span>
                  <span class="foodName">{{goods.goodsName}}</span><br>
                  <span class="foodPrice">￥{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                <li v-for="goods in typeGoods[3]" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%" height="60px"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data() {
    return {
      tableData: [],
      oftenGoods: [{ goodsId: 1, goodsName: '香辣鸡腿堡', price: 18 },
        { goodsId: 2, goodsName: '田园鸡腿堡', price: 15 },
        { goodsId: 3, goodsName: '和风汉堡', price: 15 },
        { goodsId: 19, goodsName: '超值全家桶', price: 80 },
        { goodsId: 9, goodsName: '脆皮鸡腿', price: 10 },
        { goodsId: 10, goodsName: '魔法鸡块', price: 18 },
        { goodsId: 15, goodsName: '可乐大杯', price: 6 },
        { goodsId: 16, goodsName: '鲜煮咖啡', price: 18 },
        { goodsId: 11, goodsName: '大份鸡米花', price: 12 },
        { goodsId: 12, goodsName: '香脆鸡柳', price: 16 }
      ],
      typeGoods: [
        [
          { goodsId: 1, goodsImg: "http://pic31.nipic.com/20130705/9475964_155806183143_2.jpg", goodsName: '香辣鸡腿堡', price: 18 },
          { goodsId: 2, goodsImg: "http://www.0475wm.com/uploadfile/1369336156.jpg", goodsName: '田园鸡腿堡', price: 15 },
          { goodsId: 3, goodsImg: "http://pic13.nipic.com/20110316/2419558_104511442119_2.jpg", goodsName: '和风汉堡', price: 15 },
          { goodsId: 4, goodsImg: "http://pic8.nipic.com/20100711/5224629_131437097122_2.jpg", goodsName: '麦香鱼堡', price: 12 },
          { goodsId: 5, goodsImg: "https://img1.doubanio.com/lpic/s10385277.jpg", goodsName: '吉士蛋堡', price: 16 },
          { goodsId: 6, goodsImg: "http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2018/06/201806271648252562.png", goodsName: '不素之霸', price: 18 }
        ],
        [
          { goodsId: 7, goodsImg: "http://pic28.nipic.com/20130501/6647776_125657638139_2.jpg", goodsName: '美味薯条', price: 6 },
          { goodsId: 8, goodsImg: "http://kfc.menu.xixik.net/c/mdl/xiaoyumibei.png", goodsName: '玉米杯', price: 8 },
          { goodsId: 9, goodsImg: "http://p4.qhimg.com/t01f6366e0d9b37a7cd.jpg", goodsName: '脆皮鸡腿', price: 10 },
          { goodsId: 10, goodsImg: "http://static.wanlitong.com/app_series/wanlitong/application/images/20131224102606.jpg", goodsName: '魔法鸡块', price: 18 },
          { goodsId: 11, goodsImg: "http://pic27.nipic.com/20130329/964933_121751254164_2.jpg", goodsName: '大份鸡米花', price: 12 },
          { goodsId: 12, goodsImg: "http://pic41.nipic.com/20140520/18810440_220448567103_2.jpg", goodsName: '香脆鸡柳', price: 16 },
          { goodsId: 13, goodsImg: "http://pic20.nipic.com/20120420/9301004_112313368164_2.jpg", goodsName: '葡式蛋挞', price: 16 }
        ],
        [
          { goodsId: 14, goodsImg: "http://kfc.menu.xixik.net/c/mdl/youpindoujiang.png", goodsName: '优品豆浆', price: 8 },
          { goodsId: 15, goodsImg: "http://pic5.nipic.com/20091222/185626_205203338651_2.jpg", goodsName: '可乐大杯', price: 6 },
          { goodsId: 16, goodsImg: "http://a1.att.hudong.com/36/88/19300001236771131814880828263.jpg", goodsName: '鲜煮咖啡', price: 12 },
          { goodsId: 17, goodsImg: "http://www.quanmama.com/AdminImageUpload/3657848QQ%E5%9B%BE%E7%89%8720140725113531.jpg", goodsName: '鲜果粒橙', price: 10 },
          { goodsId: 17, goodsImg: "http://static.5ikfc.com/img/mdl/menu/drink-4929_06.jpg", goodsName: '雪碧杯', price: 10 }
        ],
        [
          { goodsId: 18, goodsImg: "http://static.5ikfc.com/img/mdl/menu/happy-4929_04.jpg", goodsName: '开心乐园餐', price: 60 },
          { goodsId: 19, goodsImg: "http://jilin.sinaimg.cn/2013/1029/U9495P1387DT20131029092214.jpg", goodsName: '超值全家桶', price: 80 },
          { goodsId: 19, goodsImg: "http://pic20.photophoto.cn/20110816/0020033031088544_b.jpg", goodsName: '圣诞新春桶', price: 99 }
        ]
      ],
      totalCount: 0,
      totalMoney: 0
    }
  },

  mounted: function() {
    var orderHeight = document.body.clientHeight;
    // console.log(orderHeight);
    document.getElementById('order-list').style.height = orderHeight + 'px';
  },
  methods: {
    addOrderList(goods) {
      // this.totalMoney = 0;
      // this.totalCount = 0;
      // 商品是否存在于列表中
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //根据判断结果编写业务逻辑（有-增加数量，没有-添加到数组)
      if (isHave) {
        // 改变列表中商品数量
        let arr = this.tableData.filter(a => a.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = { goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1 };
        this.tableData.push(newGoods);
      }
      this.sumMoney();
    },
    // 删除单个商品
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(a => a.goodsId != goods.goodsId);
      this.sumMoney();
    },
    // 删除全部商品
    delAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    // 模拟结账
    checkOut() {
      if (this.totalCount != 0) {
        // 有值
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: '结账成功，感谢你又完成了一笔订单！',
          type: 'success'
        });
      } else {
        this.$message.error('购物车为空，不能结账！');
      }
    },
    // 汇总数量和金额
    sumMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        this.tableData.forEach((element) => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price * element.count);
        })
      }
    }
  }
}

</script>
<style scoped>
.pos-order {
  background-color: #F9FAFC;
  border-right: 1px solid #C0CCDA;
}

.order-btn {
  margin-top: 10px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #D3dce6;
  background-color: #F9FAFC;
  padding: 10px;
  text-align: left;
}

.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #93b5cf;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}

.o-price {
  color: #2474b5;
  font-weight: 550;
}

.goods-type {
  clear: both;
}

.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #E5E9F2;
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
  width: 90px;
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

.tab {
  padding-left: 5px;
}


.sumDiv {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #D3dce6;
}

</style>
