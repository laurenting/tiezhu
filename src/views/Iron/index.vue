<template>
  <div class="calaulate">
    <el-form label-position="left" label-width="auto" size="medium">
      <el-row v-for=" item in dj" :key="item.name">
        <el-col :xs="16" :sm="16" :lg="16">
          <el-form-item :label="item.name">
          <el-input v-model="item.value" clearable placeholder="请输入定金, 没有则无需填写"/>
        </el-form-item>
        </el-col>
        <el-col :xs="8" :sm="8" :lg="8">
          <el-form-item label="支付方式">
            <el-select v-model="item.way" placeholder="请选择支付方式" >
            <el-option
              v-for="item in lessOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-divider/>
      <el-row v-for=" item in gfk" :key="item.name">
        <el-col :xs="16" :sm="16" :lg="16">
          <el-form-item :label="item.name">
          <el-input v-model="item.value" clearable placeholder="请输入首付/购房款, 没有则无需填写"/>
        </el-form-item>
        </el-col>
        <el-col :xs="8" :sm="8" :lg="8">
          <el-form-item label="支付方式">
            <el-select v-model="item.way" placeholder="请选择支付方式" >
            <el-option
              v-for="item in moreOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-divider/>
      <el-row>
        <el-col :xs="16" :sm="16" :lg="16">
          <el-form-item label="贷款">
          <el-input v-model.number="loans.value" clearable placeholder="请输入贷款金额"/>
        </el-form-item>
        </el-col>
        <el-col :xs="8" :sm="8" :lg="8">
          <el-form-item  label="若未获批">
            <el-select v-model="loans.way" placeholder="贷款未获批的处理方式">
            <el-option
              v-for="item in noLoanslOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-divider/>
      <el-row>
        <el-col :xs="16" :sm="16" :lg="16">
          <el-form-item label="物业保证金">
          <el-input v-model.number="management.value" clearable placeholder="请输入物业保证金"/>
        </el-form-item>
        </el-col>
        <el-col :xs="8" :sm="8" :lg="8">
          <el-form-item  label="支付方式">
            <el-select v-model="management.way" clearable placeholder="请选择支付方式">
            <el-option
              v-for="item in lessOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :xs="16" :sm="16" :lg="16">
          <el-form-item label="户口保证金">
          <el-input v-model.number="household.value" placeholder="请输入户口保证金"/>
        </el-form-item>
        </el-col>
        <el-col :xs="8" :sm="8" :lg="8">
          <el-form-item  label="支付方式">
            <el-select v-model="household.way" placeholder="请选择支付方式">
            <el-option
              v-for="item in lessOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-divider/>
      <el-row>
        <el-button class="cal-button" type="text" @click="calculate">计算</el-button>
      </el-row>
    </el-form>
    <el-dialog
      title="放弃资金存管的金额为:"
      :visible.sync="centerDialogVisible"
      width="80%"
      center>
      <div class="result">{{ result }} 元</div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="centerDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Iron',
  data () {
    return {
      centerDialogVisible: false,
      dj: [{
        name: '定金 1',
        value: null,
        way: ''
      },
      {
        name: '定金 2',
        value: null,
        way: ''
      }],
      gfk: [{
        name: '购房款 1',
        value: null,
        way: ''
      }, {
        name: '购房款 2',
        value: null,
        way: ''
      }, {
        name: '购房款 3',
        value: null,
        way: ''
      }],
      loans: {
        name: '贷款',
        value: null,
        way: ''
      },
      management: {
        name: '物业保证金',
        value: null,
        way: ''
      },
      household: {
        name: '户口保证金',
        value: null,
        way: ''
      },
      result: 0,
      lessOptions: [{
        value: '银行资金存管', label: '银行资金存管'
      }, {
        value: '理房通资金存管', label: '理房通资金存管'
      }, {
        value: '自行支付', label: '自行支付'
      }],
      moreOptions: [{
        value: '银行资金存管', label: '银行资金存管'
      }, {
        value: '理房通资金存管', label: '理房通资金存管'
      }, {
        value: '房管局监管', label: '房管局监管'
      }, {
        value: '自行支付', label: '自行支付'
      }, {
        value: '第三方担保支付', label: '第三方担保支付'
      }],
      noLoanslOptions: [{
        value: '自筹房款且自行支付', label: '自筹房款且自行支付'
      }, {
        value: '违约责任/合同终止', label: '违约责任/合同终止'
      }],
      sum: 0
    }
  },
  methods: {
    calculate () {
      this.centerDialogVisible = true
      this.dj.map(item => {
        if (item.way === '自行支付') {
          this.sum += Number(item.value)
        }
      })
      this.gfk.map(item => {
        if (item.way === '自行支付' || item.way === '第三方担保支付') {
          this.sum += Number(item.value)
        }
      })
      if (this.loans.way === '自筹房款且自行支付') {
        this.sum += Number(this.loans.value)
      }
      if (this.management.way === '自行支付') {
        this.sum += Number(this.management.value)
      }
      if (this.household.way === '自行支付') {
        this.sum += Number(this.household.value)
      }
      this.result = this.sum
      this.sum = 0
    }
  }
}
</script>

<style scoped>
.result {
  text-align: center;
  font-size: 20px;
  font-weight: 800;
}
.cal-button {
  width: 100%;
  height: 100%;
  font-size: 20px;
}
</style>
