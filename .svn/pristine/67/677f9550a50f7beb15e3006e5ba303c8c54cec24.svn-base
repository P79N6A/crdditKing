<template>
    <ui-layout :brs="brs">
        <ui-container>
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-input v-model="queryMap.key" placeholder="请输入姓名或身份证" clearable></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="3">
                            <el-form-item >
                                <el-select filterable v-model="queryMap.type"  style="margin-left:10px;width:200px;" @change="getTableData">
                                    <el-option label="全部" value= '-1'></el-option>
                                    <el-option label="贷款申请" value= '1'></el-option>
                                    <el-option label="提现申请" value= '2'></el-option>
                                    <el-option label="退款申请" value= '3'></el-option>
                                </el-select>
                            </el-form-item>
                        </el-col>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-date-picker v-model="startTimeNull" @change="getSearchTime" placeholder="开始日期" style="margin-left:20px;width:87%;"></el-date-picker>
                            </el-form-item>
                        </el-col>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-date-picker v-model="endTimeNull" @change="getSearchTime" placeholder="结束日期" style="margin-left:20px;width:87%;"></el-date-picker>
                            </el-form-item>
                        </el-col>
                        <el-col :span="3">
                            <el-form-item label=" ">
                                <el-button type="primary" @click="getTableData">
                                    <i class="el-icon-search"></i>
                                    <span>查询</span>
                                </el-button>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
            </el-row>
            <el-row>
                <el-col>
                    <el-table :data="tableData" border style="width: 80%">
                        <el-table-column type="index" label="序号" align="center" ></el-table-column>
                        <el-table-column prop="nickName" label="昵称" align="center" width="120px;"></el-table-column>
                        <el-table-column prop="name" label="姓名" align="center" ></el-table-column>
                        <el-table-column prop="idCard" label="身份证" align="center" width="200px;"></el-table-column>
                        <el-table-column prop="bankNumber" label="银行卡" align="center" width="200px;"></el-table-column>
                        <el-table-column prop="issue" label="期数" align="center" ></el-table-column>
                        <el-table-column prop="interest" label="月供利息" align="center" ></el-table-column>
                        <el-table-column prop="alipayAccount" label="支付宝账号" align="center" width="150px;"></el-table-column>
                        <el-table-column prop="serviceMoney" label="服务费" align="center"></el-table-column>
                        <el-table-column prop="money" label="金额" align="center"></el-table-column>
                        <el-table-column prop="type" label="类型" width="100" align="center">
                            <template slot-scope="scope">
                                <span v-if="scope.row.type == 1">贷款申请</span>
                                <span v-if="scope.row.type == 2">提现申请</span>
                                <span v-if="scope.row.type == 3">退款申请</span>
                            </template>
                        </el-table-column>
                        <el-table-column prop="type" label="状态" width="100" align="center">
                            <template slot-scope="scope">
                                <span v-if="scope.row.status == 'Y'">同意</span>
                                <span v-if="scope.row.status == 'N'">未审核</span>
                                <span v-if="scope.row.status == 'F'">拒绝</span>
                            </template>
                        </el-table-column>
                        <el-table-column prop="createTime" label="申请时间" align="center" width="150px;">
                            <template slot-scope="scope">
                                <span>{{scope.row.createTime | dateFmt('yyyy-MM-dd')}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column prop="remark" label="备注" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column label="操作" width="300" fixed="right" align="center">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="toAgree(scope.row)" v-if="scope.row.status == 'N'" >通过</el-button>
                                <el-button type="text" size="small" @click="toReject(scope.row)" v-if="scope.row.status == 'N'" >驳回</el-button>
                                <el-button type="text" size="small" v-if="scope.row.status == 'Y'" disabled>已通过</el-button>
                                <el-button type="text" size="small" v-if="scope.row.status == 'F'" disabled>已驳回</el-button>
                                <span v-if="scope.row.status == 'F' || scope.row.status == 'Y'">
                                    <el-button type="text" size="small" disabled style="margin-left:10px;">编辑</el-button>
                                </span>
                                <span v-if="scope.row.status == 'N'">
                                    <el-button type="text" size="small" @click="toEdit(scope.row)" style="margin-left:10px;">编辑</el-button>
                                </span>
                            </template>
                        </el-table-column>
                    </el-table>
                </el-col>
            </el-row>

            <el-dialog  title="备注" :visible.sync="dialogVisibleRemark" width="20%" :before-close="closeRemark">
                                <span>
                                    <el-form :inline="true" :model="remarkMap" ref="remarkMap"  class="demo-form-inline" label-position="left" label-width="150px">
                                        <el-col>
                                            <el-row>
                                              <el-form-item label="添加备注信息">
                                                        <div >
                                                            <el-input v-model="remarkMap.remark" autocomplete="off" style="width:300px;"  type="textarea" :rows="3">
                                                            </el-input>
                                                        </div>
                                                </el-form-item>
                                            </el-row>
                                        </el-col>
                                    </el-form>
                                </span>
                                <span slot="footer" class="dialog-footer">
                                    <el-button type="primary" @click="remarkConfirm">确 定</el-button>
                                </span>


                                
                </el-dialog>


            <el-row>
                <ui-page :total="total" :pageSize="10" @size-change="handleSizeChange" @current-change="handleCurrentChange"></ui-page>
            </el-row>
        </ui-container>
    </ui-layout>
</template>

<script>
import UiPage from "../../components/UiPage.vue";
import { Loading } from "element-ui";
import { exportExcel } from "../../../config/project/project.env";
export default {
  components: { UiPage, Loading },
  data() {
    return {
      exportExcel:exportExcel,
      brs: [{ name: "贷款管理", to: { name: "loansManager" } }],
      total: 100,
      startTimeNull: null,
      endTimeNull: null,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        key: "",
        startTime: null,
        endTime: null,
        type:'-1'
      },
      remarkMap:{
        id:null,
        remark:null,
        status:null,
        memberId:null,
        type:null,
        money:null
      },
      tableData: [],
      dialogVisibleRemark: false,
    };
  },
  mounted() {
    this.getTableData();
  },
  computed: {
    
  },
  methods: {
    toEdit(row) {
      this.$to({ name: "loansEdit", query: row });
    },
      closeRemark(done){
        this.remarkMap.id = null;
        this.remarkMap.remark = null;
        this.remarkMap.status = null;
        this.remarkMap.type = null;
        this.remarkMap.money = null;
        done();
    },
    //同意
    toAgree(row){
        this.remarkMap.id = row.id;
        this.remarkMap.status = 'Y';
        this.remarkMap.memberId = row.memberId;
        this.remarkMap.type = row.type;
        this.remarkMap.money = row.money;
        this.dialogVisibleRemark = true;
        //   this.$post("loansSave", { id: row.id, status: 'Y' }).then(res => {
        //     this.getTableData();
        //     this.$message({ 
        //         message: "操作成功", 
        //         type: "success",
        //         center:true 
        //     });
        //   });
    },
    //拒绝
    toReject(row){
        this.remarkMap.id = row.id;
        this.remarkMap.status = 'F';
        this.remarkMap.memberId = row.memberId;
        this.remarkMap.type = row.type;
        this.remarkMap.money = row.money;
        this.dialogVisibleRemark = true;
        //   this.$post("loansSave", { id: row.id, status: 'F' }).then(res => {
        //     this.getTableData();
        //     this.$message({ 
        //         message: "操作成功", 
        //         type: "success",
        //         center:true 
        //     });
        //   });
    },
    //确定添加备注触发
    remarkConfirm() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("loansSave", this.remarkMap)
        .then(res => {
          this.dialogVisibleRemark = false;
          this.$message({
            showClose: true,
            message: "审核成功",
            type: "success",
            center: true
          });
          loadingInstance.close();
          this.getTableData();
        })
        .catch(() => {
          loadingInstance.close();
        });
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("loansList", this.queryMap)
        .then(res => {
          this.tableData = res.list;
          this.total = res.total;
          loadingInstance.close();
        })
        .catch(() => {
          loadingInstance.close();
        });
    },
    //时间搜索
    getSearchTime() {
      this.queryMap.startTime = this.$getYMD(this.startTimeNull);
      this.queryMap.endTime = this.$getYMD(this.endTimeNull);
    },
    handleSizeChange(val) {
      this.queryMap.pageNo = val;
      this.getTableData();
    },
    handleCurrentChange(val) {
      this.queryMap.pageNo = val;
      this.getTableData();
    }
  }
};
</script>

<style>
@import "../../style/teaend.less";
</style>