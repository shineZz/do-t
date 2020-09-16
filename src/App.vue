<template>
  <div id="app">
    <h1>盈利计算</h1>
    <div>
      <el-form label-position="right" :inline="true" label-width="80px" :model="form">
        <el-form-item label="买入价">
          <el-input-number
            v-model="form.val0"
            controls-position="right"
            :min="0.001"
            :max="20000"
            :step="0.01"
          ></el-input-number>
        </el-form-item>
        <el-form-item label="是否计算费用" label-width="110px">
          <el-switch v-model="form.state"></el-switch>
        </el-form-item>
        <el-form-item label="卖出价">
          <el-input-number
            v-model="form.val1"
            controls-position="right"
            :min="0.001"
            :max="20000"
            :step="0.01"
          ></el-input-number>
        </el-form-item>
        <el-form-item label="数量">
          <el-input-number
            v-model="form.num"
            controls-position="right"
            :min="100"
            :max="10000000000"
            :step="100"
          ></el-input-number>
        </el-form-item>
        <el-form-item label="佣金费率">
          <el-input-number
            v-model="form.val2"
            controls-position="right"
            :min="0.01"
            :max="3"
            :step="0.01"
          ></el-input-number>
        </el-form-item>
      </el-form>
      <el-button type="primary" @click="handelClick(0)">计算盈利</el-button>
      <!-- <el-button type="primary" @click="handelClick(1)">做T</el-button> -->
    </div>
    <div style="margin-top:30px" class="card-wrap">
      <div style="margin-bottom:30px">
        <el-card class="box-card">
          <p v-show="!result">盈利结果</p>
          <p>{{result}}</p>
        </el-card>
      </div>
      <el-card class="box-card">
        <div class="card-sm">
          提示：
          <p>1、计算盈利，必须输入买入价格，卖出价格，数量。</p>
          <p>2、是否计入手续费的意思是如果计入，买入价不再而外计算手续费，如果选否，那么计算的时候会继续按照佣金费率扣除费用</p>
          <p>3、佣金费率默认万分之2.5。</p>
          <p>4、印花税统一为1‰ 。</p>
          <!-- <p>5、做T，最少输入买入价格,数量，卖出价格三选2</p> -->
        </div>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      form: {
        val0: 1,
        val1: 1,
        num: 100,
        val2: 2.5,
        state: true
      },
      result: ''
    }
  },
  methods: {
    handelClick(type) {
      let { val0, val1, num, val2, state } = this.form
      if (!val0||!val1||!num) {
        this.$message({
          message: '请按照提示要求输入',
          type: 'warning'
        })
        return false
      }
      let brokerage0
      let brokerage1
      let result
      let cost
      if (type === 0) {
        brokerage0 = (val0 * num * val2) / 10000 > 5 ? (val0 * num * val2) / 10000 : 5
        brokerage1 = (val1 * num * val2) / 10000 > 5 ? (val1 * num * val2) / 10000 : 5

        if (state) {
          cost = brokerage1 + (val1 * num * 1) / 1000
          result = val1 * num - val0 * num - cost
        } else {
          cost = brokerage0 + brokerage1 + (val1 * num * 1) / 1000
          result = val1 * num - val0 * num - cost
        }
        this.result = `盈利为${result}
        费用为${cost}`
      } else {
        // if (state) {
        //   bool = (val1 * 100 * val2) / 10000 > 5
        //   let resultVal1
        //   if (bool) {
        //     resultVal1 = (10000 * val0) / (10000 - 10 - val2)
        //   } else {
        //     if (val1 || num) {
        //       if (val1) {
        //         ;(val1 * num * val2) / 10000 == 5
        //         ;(val1 * num) / 1000
        //       }
        //     } else {
        //       this.$message({
        //         message: '请按照提示要求输入',
        //         type: 'warning'
        //       })
        //     }
        //   }

        //   // let resultNum = (999 / 200) * val0
        //   // let resultVal1 = 999 / (40 * val0 * val2)
        // } else {
        //   let resultNum = (999 / 200) * val0
        //   let resultVal1 = 999 / (40 * val0 * val2)
        //   cost = brokerage0 + brokerage1 + (val1 * num * 1) / 1000
        //   result = val1 * num - val0 * num - cost
        // }
        console.log(111)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #606266;
  margin-top: 60px;
}
</style>
<style lang="scss" scoped>
.card-wrap {
  width: 60%;
  margin: 0 auto;
}
.card-sm {
  p {
    font-size: 16px;
    color: #303133;
    font-weight: 400;
  }
}
</style>