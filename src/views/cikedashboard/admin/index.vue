<template>
  <div class="dashboard-editor-container">
     <github-corner class="github-corner" />

    <leads-table :tableData="tableData"></leads-table>
    <panel-group :leadsSelldata="leadsSellData" @handleSetLineChartData="handleSetLineChartData" />

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>

    <el-row :gutter="32">
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <raddar-chart />
        </div>
      </el-col>
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <pie-chart />
        </div>
      </el-col>
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <bar-chart />
        </div>
      </el-col>
    </el-row>

    <el-row :gutter="8">
      <el-col :xs="{span: 24}" :sm="{span: 24}" :md="{span: 24}" :lg="{span: 12}" :xl="{span: 12}" style="padding-right:8px;margin-bottom:30px;">
        <transaction-table />
      </el-col>
      <el-col :xs="{span: 24}" :sm="{span: 12}" :md="{span: 12}" :lg="{span: 6}" :xl="{span: 6}" style="margin-bottom:30px;">
        <todo-list />
      </el-col>
      <el-col :xs="{span: 24}" :sm="{span: 12}" :md="{span: 12}" :lg="{span: 6}" :xl="{span: 6}" style="margin-bottom:30px;">
        <box-card />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import GithubCorner from '@/components/GithubCorner'
import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'
import RaddarChart from './components/RaddarChart'
import PieChart from './components/PieChart'
import BarChart from './components/BarChart'
import TransactionTable from './components/TransactionTable'
import TodoList from './components/TodoList'
import BoxCard from './components/BoxCard'
import axios from 'axios'
import LeadsTable from './components/LeadsTable'


//let lineChartDataS={}
 let lineChartDataS = 
{
  newVisitis: {
    expectedData: [],
    actualData: []
  },
  messages: {
    expectedData: [],
    actualData: []
  },
  purchases: {
    expectedData: [],
    actualData: []
  },
  shoppings: {
    expectedData: [],
    actualData: []
  },
  leadsSellData: {
          totalLeads: 0,
          effectiveLeads: 0,
          salesData: 0,
          orderAmount: 0
  }
}
let tableData = []

// let tableData = [{
//                     leadsId: "",
//                     date: "",
//                     contactDate: "",
//                     phoneNumber: "",
//                     name: "",
//                     address: "",
//                     sex: "",
//                     age: "",
//                     education: "",
//                     city: "",
//                     contact: "",
//                     consultant: "",
//                     source: "",
//                     source2: "",
//                     branch: "",
//                     status: "",
//                     status2: "",
//                     remark: "",
//                     orderAmount: "",
//                     orders: ""
//                 }]
                
// let  newVisitis = {
//     expectedData: [],
//     actualData: []
//   }


export default {
  name: 'DashboardAdmin',
  components: {
    GithubCorner,
    PanelGroup,
    LineChart,
    RaddarChart,
    PieChart,
    BarChart,
    TransactionTable,
    TodoList,
    BoxCard,
    LeadsTable
},
  data() {
    return {

      lineChartData: lineChartDataS,
      // lineChartData:null,
       //newVisitis: lineChartDataS.newVisitis,
       leadsSellData: lineChartDataS.leadsSellData,
       tableData: tableData
      //lineChartData: lineChartData.newVisitis
    }
  },
  methods: {
    handleSetLineChartData(type) {
      this.lineChartData = lineChartDataS[type]
    },
  },
  created() {
    axios.get('http://localhost:3000/leadsData').then(res => {
      lineChartDataS = res.data.lineChartData
      this.lineChartData = res.data.lineChartData.newVisitis
      this.tableData = res.data.tableData
      //this.newVisitis = res.data.lineChartData.newVisitis
      this.leadsSellData = res.data.lineChartData.leadsSellData
      console.log(res.data.lineChartData)

     
    })
    //console.log(lineChartDataS)
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
