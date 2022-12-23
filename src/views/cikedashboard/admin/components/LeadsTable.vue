<template>
    <div>
   
    <div class="block">
        <el-button @click="resetDateFilter">清除日期过滤器</el-button>
    <el-button @click="clearFilter">清除所有过滤器</el-button>

    <el-date-picker
      v-model="value1"
      type="daterange"
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期">
    </el-date-picker>
    
    
  </div>
  <div class="block">
       
            
       <el-checkbox-group v-model="checkboxVal">
        
         <el-checkbox label="sex">
           性别
         </el-checkbox>
         <el-checkbox label="age">
           年龄
         </el-checkbox>
         <el-checkbox label="education">
           教育
         </el-checkbox>
         <el-checkbox label="city">
           城市
         </el-checkbox>
         <el-checkbox label="address">
           地址
         </el-checkbox>
         <el-checkbox label="contact">
           联系方式
         </el-checkbox>
         
         <el-checkbox label="remark">
           备注
         </el-checkbox>
       </el-checkbox-group>
      
        <el-checkbox-group v-model="salesVal">
         <el-checkbox label="orderAmount">
           签单总金额
         </el-checkbox>
         <el-checkbox label="orders">
           签单笔数
         </el-checkbox>
        </el-checkbox-group>
         
     </div>
      
    
    <el-table
      ref="filterTable"
      :data="tableData"
      max-height="700"
      :key="key"
      stripe
      border
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      :default-sort = "{prop: 'date', order: 'descending'}">


      

    
   
      <el-table-column
      type="selection"
      width="35">
    </el-table-column>
    <el-table-column type="expand" label="跟进" width="35">
      <template slot-scope="props">
        <el-form label-position="left" inline class="demo-table-expand">
          <el-form-item label="商品名称">
            <span>{{ props.row.date }}</span>
          </el-form-item>
          <el-form-item label="所属店铺">
            <span>{{ props.row.name }}</span>
          </el-form-item>
          <el-form-item label="商品 ID">
            <span>{{ props.row.address }}</span>
          </el-form-item>
          <el-form-item label="店铺 ID">
            <span>{{ props.row.tag }}</span>
          </el-form-item>
        </el-form>
      </template>
    </el-table-column>
    <el-table-column
        prop="leadsId"
        label="编号"
        width="70"
        sortable>
        <template slot-scope="props">
         <el-button @click="drawer = true" type="text" >
             {{props.row.leadsId}} 
            
</el-button> 
</template>
      </el-table-column>
      <el-table-column
        prop="date"
        label="日期"
        width="100"
        sortable
        column-key="date"
        :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
      :filter-method="filterHandler"
>
      </el-table-column>
      <el-table-column
        prop="contactDate"
        label="联系日期"
        width="100"
        sortable
        column-key="contactDate"
        :filters="[{text: '2022-05-01', value: '2022-05-01'}, {text: '20121-05-02', value: '2021-05-02'}, {text: '2021-05-03', value: '2021-05-03'}, {text: '2022-05-04', value: '2022-05-04'}]"
      :filter-method="filterHandler"
>
      </el-table-column>

      <el-table-column
        prop="phoneNumber"
        label="电话号码"
        width="120"
        sortable
        column-key="phoneNumber"
        :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
      :filter-method="filterHandler"
>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="90"
        sortable>
        <template slot-scope="scope">

           

        <el-popover trigger="hover" placement="top">
          <p>性别: {{ scope.row.sex }}&nbsp; &nbsp; &nbsp;
             &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 
             &nbsp; 年龄: {{ scope.row.age }}</p>
          <p>学历: {{ scope.row.education }}</p>
          <p>城市: {{ scope.row.city }}</p>
          <p>地址: {{ scope.row.address }}</p>
          <p>联系方式: {{ scope.row.contact }}</p>
          <p>备注: {{ scope.row.remark }}</p>
          <div slot="reference" class="name-wrapper">
            {{ scope.row.name }}
          </div>
        </el-popover>
      </template>
      
      </el-table-column>
      <el-table-column v-for="fruit in formThead" :key="fruit" :label="fruit">
        <template slot-scope="scope">
          {{ scope.row[fruit] }}
        </template>
      </el-table-column>
      <el-table-column
        prop="consultant"
        label="顾问"
        width="100"
        sortable
        column-key="consultant"
        :filters="[{ text: 'Tina', value: 'Tina' }, { text: 'Cassie', value: 'Cassie' }, { text: 'Jasmine', value: 'Jasmine' }]"
      :filter-method="filterConsultant"
      filter-placement="bottom-end">
        
      <template slot-scope="scope">
        <el-tag> {{scope.row.consultant}}</el-tag>
      </template>


      </el-table-column>
    
      <el-table-column
      prop="source"
      label="来源"
      width="100"
      sortable
      :filters="[{ text: '模考网站', value: '模考网站' }, { text: '大众', value: '大众' }, { text: '百度', value: '百度' }]"
      :filter-method="filterSource"
      filter-placement="bottom-end">
    </el-table-column>

    <el-table-column
      prop="source2"
      label="二级来源"
      sortable
      width="120"
      :filters="[{ text: '试听课', value: '试听课' }, { text: '预约咨询', value: '预约咨询' }, { text: '在线咨询', value: '在线咨询' }, { text: '留电话', value: '留电话' }, { text: '团购', value: '团购' }, { text: '作文批改', value: '作文批改' }, { text: '口语', value: '口语' }]"
      :filter-method="filterSource2"
      filter-placement="bottom-end">
    </el-table-column>

    <el-table-column
      prop="branch"
      label="机构"
      width="100"
      sortable
      :filters="[{ text: '线上', value: '线上' }, { text: '高新', value: '高新' },{ text: '东门', value: '东门' }]"
      :filter-method="filterBranch"
      filter-placement="bottom-end">
      <template slot-scope="scope">
        <el-tag
          :type="scope.row.branch === '线上' ? 'primary' : 'success'"
          disable-transitions>{{scope.row.branch}}</el-tag>
      </template>
    </el-table-column>

      <el-table-column
      prop="status"
      label="状态"
      width="100"
      sortable
      :filters="[{ text: '已报名', value: '已报名' }, { text: '已跟进', value: '已跟进' }, { text: '无效', value: '无效' },{ text: '未跟进', value: '未跟进' }]"
      :filter-method="filterStatus"
      filter-placement="bottom-end">
      <template slot-scope="scope">
        <el-tag
          :type="scope.row.status === '未跟进' ? 'primary' : 'success'"
          disable-transitions>{{scope.row.status}}</el-tag>
      </template>
    </el-table-column>

    <el-table-column
        prop="status2"
        label="二级状态"
        width="120"
        sortable
        :filters="[{ text: '可谈续报', value: '可谈续报' }, { text: '不谈续报', value: '不谈续报' }, { text: '无效联系方式', value: '无效联系方式' },{ text: '无意向', value: '无意向' },{ text: '放弃跟进', value: '放弃跟进' },{ text: '需求不对口', value: '需求不对口' },{ text: '没来得及', value: '没来得及' },{ text: '工作安排', value: '工作安排' },{ text: '约线上试听', value: '约线上试听' },{ text: '邀约到校', value: '邀约到校' },{ text: '约线下试听', value: '约线下试听' },{ text: '谈报名', value: '谈报名' },{ text: '近期跟进', value: '近期跟进' },{ text: '长期跟进', value: '长期跟进' }]"
        :filter-method="filterStatus2"
        filter-placement="bottom-end">

      </el-table-column>
      <!-- <el-table-column
        prop="orderAmount"
        label="签单总金额"
        width="120"
        sortable>

      </el-table-column>
      <el-table-column
        prop="orders"
        label="签单笔数"
        width="120"
        sortable>
        <template slot-scope="scope">
          {{ scope.row.orders }}
        </template>
        
      </el-table-column> -->
      <el-table-column v-for="salesCol in formSales"  :label="salesCol"
       sortable>
        <template slot-scope="scope">
            <el-button @click="drawer = true" type="text" >
          {{ scope.row[salesCol] }}
        </el-button> 
        </template>
      </el-table-column>



      
    </el-table>
    
  <div class="block">
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="100"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400">
    </el-pagination>
  </div>
  <el-drawer
  title="客资跟进记录"
  :visible.sync="drawer"
  size="70%">
  <div>
    <el-table 
    :data="contactData"
      stripe
      border
      max-height="700"
      tooltip-effect="dark"
      style="width: 100%">
        <el-table-column  
        prop="contactTime"
        width="120"
        label="跟进时间">

        </el-table-column>
        <el-table-column  
        prop="contacter"
        width="80"
        label="跟进人">

        </el-table-column>
        <el-table-column  
        prop="contactDetail"
        label="跟进内容">

        <template slot-scope="{row}">
          <template v-if="row.edit">
            <el-input v-model="row.title" class="edit-input" size="small" />
            <el-button
              class="cancel-btn"
              size="small"
              icon="el-icon-refresh"
              type="warning"
              @click="cancelEdit(row)"
            >
              cancel
            </el-button>
          </template>
          <span v-else>{{ row.contactDetail }}</span>
        </template>

        </el-table-column>
        <el-table-column align="center" label="Actions" width="120">
        <template slot-scope="{row}">
          <el-button
            v-if="row.edit"
            type="success"
            size="small"
            icon="el-icon-circle-check-outline"
            @click="confirmEdit(row)"
          >
            Ok
          </el-button>
          <el-button
            v-else
            type="primary"
            size="small"
            icon="el-icon-edit"
            @click="row.edit=!row.edit"
          >
            Edit
          </el-button>
        </template>
      </el-table-column>
    </el-table>
</div>
  <div>
   <el-button @click="innerDrawer = true">打开里面的!</el-button>
   <el-drawer
     title="我是里面的"
     :append-to-body="true"
     :before-close="handleClose"
     :visible.sync="innerDrawer">
     <p>_(:зゝ∠)_</p>
   </el-drawer>
  </div>
</el-drawer>
</div>

  </template>
  
  <script>
import InlineEditTable from '@/views/table/inline-edit-table.vue';
import { array } from 'jszip/lib/support';
  const defaultFormThead = []
  const defaultFormSales = ["orderAmount", "orders"]
    export default {
    data() {
        return {
            drawer: false,
            innerDrawer: false,
            pickerOptions: {
                shortcuts: [{
                        text: "最近一周",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                            picker.$emit("pick", [start, end]);
                        }
                    }, {
                        text: "最近一个月",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                            picker.$emit("pick", [start, end]);
                        }
                    }, {
                        text: "最近三个月",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                            picker.$emit("pick", [start, end]);
                        }
                    }]
            },
            value1: "",
            value2: "",
            currentPage1: 5,
            currentPage2: 5,
            currentPage3: 5,
            currentPage4: 4,
            contactData: [{
                contactTime: "202210221965",
                contacter: "Tina",
                contactDetail: "12.30号 cindy去电，初高中的英语水平，住在攀成钢那边，现在没有上班，时间比较闲，可以上课的，学习英语主要是自我提升和旅行方面，说团购了我们的试听券，询问是只想要试听一下课程呢，还是说考虑学习呢，说想要先试听看看课程，可以的话再报班学习，明天下午有时间，预约了明天下午13：30到校，先了解课程，可以试听一下L1的发音课，微信上强调了明天下午那堂课是我们非常系统的词汇发音课程",
            },{
                contactTime: "202210231965",
                contacter: "Tina",
                contactDetail: "12.31号Ivy咨询，38岁，看起来比较果断的女士，说什么还是挺喜欢的笑的，现在没有上班，时间非常灵活，主要是提升一下自己的听说水平，比如旅游的时候可以用到，没有特别具体的学习目标。了解过程当中告知自己儿子在英孚学习，询问效果怎么样的时候，回复说感觉不怎么样，但是不是特别想继续往下聊，所以重点讲了一些我们跟英孚不太一样的地方，强调了我们的线下课，感觉还是挺好的，但是想了解一下我们具体的上课模式，报名方案：L1+L2，6980，L1和L2的课程穿插上课，赠送3张半月卡",
            },{
                contactTime: "202210241965",
                contacter: "Cassie",
                contactDetail: "注意事项：现在是先L1和L2级别穿插上课，但是如果之后觉得强度比较大无法适应可调整为线上L1再上L2",
            },{
                contactTime: "202210251965",
                contacter: "Cassie",
                contactDetail: "1.5号微信提醒到校上课，说今天就不来",
            }
        ],
            tableData: [{
                    leadsId: "20221011183559",
                    date: "2016-05-03",
                    contactDate: "2022-05-03",
                    phoneNumber: "13350098761",
                    name: "王小虎",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    city: "上海",
                    contact: "email:476654309@qq.com",
                    consultant: "Tina",
                    source: "百度",
                    source2: "留电话",
                    branch: "线上",
                    status: "已报名",
                    status2: "谈报名",
                    remark: "",
                    orderAmount: "",
                    orders: ""
                }, {
                    leadsId: "20111011183559",
                    date: "2016-05-02",
                    contactDate: "2022-05-03",
                    name: "王小虎",
                    phoneNumber: "13350098761",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    city: "上海",
                    contact: "email:476654309@qq.com",
                    consultant: "Tina",
                    source: "百度",
                    source2: "在线咨询",
                    branch: "线上",
                    status: "已报名",
                    status2: "谈报名",
                    remark: "测试备注",
                    orderAmount: "12980",
                    orders: "2"
                }, {
                    date: "2016-05-04",
                    contactDate: "2022-05-03",
                    leadsId: "20131011183559",
                    name: "王小虎",
                    phoneNumber: "13350098761",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    city: "上海",
                    contact: "email:476654309@qq.com",
                    consultant: "Cassie",
                    source: "大众",
                    source2: "在线咨询",
                    branch: "锦江",
                    status: "已跟进",
                    status2: "谈报名",
                    remark: "测试备注",
                    orderAmount: "12980",
                    orders: "2"
                }, {
                    date: "2016-05-01",
                    contactDate: "2022-05-03",
                    leadsId: "20211011183559",
                    name: "王小虎",
                    phoneNumber: "13350098761",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    city: "上海",
                    contact: "email:476654309@qq.com",
                    consultant: "Cassie",
                    source: "大众",
                    source2: "试听课",
                    branch: "高新",
                    status: "无效",
                    status2: "谈报名",
                    remark: "测试备注",
                    orderAmount: "12980",
                    orders: "2"
                }, {
                    date: "2016-05-08",
                    contactDate: "2021-05-03",
                    leadsId: "20161011183559",
                    name: "王小虎",
                    phoneNumber: "13350098761",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    city: "上海",
                    contact: "email:476654309@qq.com",
                    consultant: "Cassie",
                    source: "模考网站",
                    source2: "试听课",
                    branch: "东大街",
                    status: "无效",
                    status2: "谈报名",
                    remark: "测试备注",
                    orderAmount: "22980",
                    orders: "3"
                }, {
                    leadsId: "20121011183559",
                    date: "2016-05-06",
                    contactDate: "2021-05-03",
                    name: "王小虎",
                    phoneNumber: "13350098761",
                    address: "普陀区金沙江路 1518 弄",
                    sex: "男",
                    age: "18",
                    education: "高三下期，马上毕业",
                    consultant: "Cassie",
                    source: "模考网站",
                    branch: "高新",
                    city: "上海",
                    contact: "",
                    status: "未跟进",
                    status2: "谈报名",
                    remark: "测试备注",
                    orderAmount: "12980",
                    orders: "2"
                }],
            key: 1,
            formTheadOptions: ["sex", "age", "education", "city", "address", "contact", "remark"],
            formSalesOptions: ["orderAmount", "orders"],
            orderAmount: false,
            orders: false,
            salesVal: defaultFormSales,
            checkboxVal: defaultFormThead,
            formThead: defaultFormThead,
            formSales: defaultFormSales,
            multipleSelection: []
        };
    },
    props: {
        tableData: {
      type: array,
      default(){
        return []
      }
    }
  },
    methods: {
        handleClose(done) {
            this.$confirm("还有未保存的工作哦确定关闭吗？")
                .then(_ => {
                done();
            })
                .catch(_ => { });
        },
        resetDateFilter() {
            this.$refs.filterTable.clearFilter("date");
        },
        clearFilter() {
            this.$refs.filterTable.clearFilter();
        },
        formatter(row, column) {
            return row.address;
        },
        filterSource(value, row) {
            return row.source === value;
        },
        filterSource2(value, row) {
            return row.source2 === value;
        },
        filterBranch(value, row) {
            return row.branch === value;
        },
        filterConsultant(value, row) {
            return row.consultant === value;
        },
        filterStatus(value, row) {
            return row.status === value;
        },
        filterStatus2(value, row) {
            return row.status2 === value;
        },
        filterHandler(value, row, column) {
            const property = column["property"];
            return row[property] === value;
        },
        toggleSelection(rows) {
            if (rows) {
                rows.forEach(row => {
                    this.$refs.multipleTable.toggleRowSelection(row);
                });
            }
            else {
                this.$refs.multipleTable.clearSelection();
            }
        },
        handleSelectionChange(val) {
            this.multipleSelection = val;
        },
        handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
        },
        cancelEdit(row) {
      row.title = row.originalTitle
      row.edit = false
      this.$message({
        message: 'The title has been restored to the original value',
        type: 'warning'
      })
    },
    confirmEdit(row) {
      row.edit = false
      row.originalTitle = row.title
      this.$message({
        message: 'The title has been edited',
        type: 'success'
      })
    }
    },
    watch: {
        checkboxVal(valArr) {
            this.formThead = this.formTheadOptions.filter(i => valArr.indexOf(i) >= 0);
            this.key = this.key + 1; // 为了保证table 每次都会重渲 In order to ensure the table will be re-rendered each time
        },
        salesVal(valArr) {
            this.formSales = this.formSalesOptions.filter(i => valArr.indexOf(i) >= 0);
            this.key = this.key + 1; // 为了保证table 每次都会重渲 In order to ensure the table will be re-rendered each time
        }
    },
    components: { InlineEditTable }
}

  </script>
  <style>
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 90px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 50%;
  }

.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;}
</style>