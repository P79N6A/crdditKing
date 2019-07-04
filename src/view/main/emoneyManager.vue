<template>
    <ui-layout :brs="brs">
        <ui-container style="height:50rem;overflow-y:scroll">
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-input v-model="queryMap.key" placeholder="請輸入名稱" clearable></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label=" ">
                                <el-button type="primary" @click="getTableData">
                                    <i class="el-icon-search"></i>
                                    <span>查詢</span>
                                </el-button>
                                <el-button type="primary" @click="moneyInsert">
                                    <i class="el-icon-circle-plus-outline"></i>
                                    <span>新增</span>
                                </el-button>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
            </el-row>
            <el-row>
                <el-col>
                    <el-table :data="tableData" border style="width: 80%">
                        <el-table-column type="index" label="序號" align="center" ></el-table-column>
                        <el-table-column prop="url" label="電子貨幣包圖片"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.url" :src="scope.row.url" style="width:150px;height:50px;">
                          </template>
                        </el-table-column>
                        <el-table-column prop="banner" label="專輯頁面圖片"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.banner" :src="scope.row.banner" style="width:150px;height:50px;">
                          </template>
                        </el-table-column>
                        <el-table-column prop="name" label="名稱" align="center" ></el-table-column>
                        <el-table-column prop="createTime" label="創建時間" align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.createTime | dateFmt('yyyy-MM-dd')}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column label="操作" width="300" fixed="right" align="center">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="toDelete(scope.row)" v-text="scope.row.deleted == 'N' ? '删除' : '已删除'"></el-button>
                                <el-button type="text" size="small" @click="toEdit(scope.row)" style="margin-left:10px;">编辑</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </el-col>
            </el-row>

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
      brs: [{ name: "電子貨幣包", to: { name: "emoneyManager" } }],
      total: 100,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        key: "",
      },
      memberMap:{
        id:null,
        amount:null
      },
      tableData: [],
      dialogVisible:false
    };
  },
  mounted() {
    this.getTableData();
  },
  computed: {
    
  },
  methods: {
    //跳转新增页
    moneyInsert() {
      this.$to({ name: "eMoneyInsert" });
    },
    //编辑
    toEdit(row){
        this.$to({ name: "eMoneyDetail", query: {id:row.id} });
    },
    toDelete(row){
      let visibtext = null;
      let deleted = null;
      if (row.deleted == "N") {
        visibtext = "删除";
        deleted = "Y";
      }
      if (row.deleted == "Y") {
        visibtext = "恢复";
        deleted = "N";
      }
      this.$confirm("是否确认对该电子货币包进行" + visibtext + "操作", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
          this.$post("eMoneySave", { id: row.id, deleted: deleted }).then(res => {
            this.getTableData();
            this.$message({ message: "操作成功", 
                            type: "success",
                            center:true });
          });
    }).catch(() => {});
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("eMoneyList", this.queryMap)
        .then(res => {
          this.tableData = res.list;
          this.total = res.total;
          loadingInstance.close();
        })
        .catch(() => {
          loadingInstance.close();
        });
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