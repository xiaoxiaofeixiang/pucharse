<template>
    <div class="app-container">
        <div>测试练习computed属性 action你可以实现这个计算属性的使用，这个学会之后vuex就学习的差不多</div>
        <p>数据显示{{aplus}}</p>
        <p>信息显示{{aChange}}</p>
        <button @click="updateData()">修改aplus的值</button>
        <p style="color: #2d8ac7;font-weight: bold">背包价格: {{price|formatMoney}}</p>
        <zujian :parameter1="a" :parameter2="b" @adata="getData"></zujian>
        <h1>实现购物车功能</h1>
        <div>
            <input type='checkbox' @click='checkAll()' v-model='allChecked'>全选
            <div v-for="(item,index) in productInfo">
                <input type="checkbox" :value="index" v-model="indexs" @click="changeCheckbox(index)">
                产品名称：<span>{{item.productName}}</span> <span @click="reduce(item)">-</span><input type="text" v-model="item.number" disabled size="1"><span @click="add(item)">+</span> 价格总额 <span>{{item.number*item.price}}</span>
            </div>
            总价格：<span>{{totalPrice}}</span>
        </div>
    </div>
</template>
<script>
  import zujian from '@/components/testComponents/index'
  export default {
    components: { zujian },
    data() {
      return {
        productInfo: [
          {
            productName: '加多宝',
            price: 10,
            number: 1
          },
          {
            productName: '可乐',
            price: 20,
            number: 2
          },
          {
            productName: '鲜果粒',
            price: 30,
            number: 3
          },
          {
            productName: '营养快线',
            price: 5,
            number: 3
          },
          {
            productName: '娃哈哈',
            price: 6,
            number: 7
          },
          {
            productName: '芬达',
            price: 5,
            number: 9
          }
        ],
        a: 1,
        price: 100,
        b: {
          name: 'haoxiaoxiao',
          sex: 'female'
        },
        indexs: [],
        allChecked: false,
        totalPrice: 0
      }
    },
    // 过滤属性
    filters: {
      formatMoney: function(value) {
        return '￥' + value.toFixed(2)
      }
    },
    // 计算属性
    computed: {
      aChange: function() {
        console.log(1111)
        return this.a + 2
      },
      aplus: {
        get: function() {
          console.log(666)
          return this.a + 8
        },
        set: function(value) {
          console.log('setData' + value)
          this.a = 88
        }
      }
    },
    mounted() {
      console.log('messageShow')
    },
    methods: {
      updateData() {
        this.aplus = this.aplus
      },
      getData: function(data) {
        console.log(1 + data)
      },
      // 购物车数量增加
      add(product) {
        product.number++
        this.getAllprice()
      },
      reduce(product) {
        if (product.number > 1) {
          product.number--
        }
        this.getAllprice()
      },
      checkAll: function() {
        var _this = this
        if (!_this.allChecked) {
          _this.indexs = _this.productInfo.map(function(json, index) {
            return index
          })
          console.log(_this.indexs.length)
          _this.getAllprice()
        } else {
          _this.totalPrice = 0
          _this.indexs = []
        }
      },
      getAllprice: function() {
        var that = this
        that.totalPrice = 0
        this.productInfo.map(function(item, index) {
          that.totalPrice += item.number * item.price
        })
      },
      // 取消某一个按钮
      changeCheckbox(index) {
        if (this.indexs.indexOf(index) === -1 && this.allChecked === false) {
          this.indexs.push(index)
          console.log(this.indexs)
          if (this.indexs.length === this.productInfo.length) {
            this.allChecked = true
          }
          this.totalPrice += this.productInfo[index].number * this.productInfo[index].price
        } else if (this.allChecked === true) {
          console.log(666)
          this.indexs.splice(index, 1)
          this.totalPrice -= this.productInfo[index].number * this.productInfo[index].price
          console.log(this.indexs)
        } else {
          // 计算减时候的价格
          console.log(7777)
          var index1 = this.indexs.indexOf(index)
          if (index1 > -1) {
            this.indexs.splice(index1, 1)
          }
          this.totalPrice -= this.productInfo[index].number * this.productInfo[index].price
          console.log(this.indexs)
        }
        if (this.indexs.length < this.productInfo.length) {
          this.allChecked = false
        }
      }
    }
  }
</script>
<style lang="scss" scoped>

</style>
