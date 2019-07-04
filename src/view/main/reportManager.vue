<template>
    <ui-layout :brs="brs">
        <ui-container>
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-input v-model="queryMap.key" placeholder="請輸入要查詢的內容" clearable></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label=" ">
                                <el-button type="primary" @click="getTableData">
                                    <i class="el-icon-search"></i>
                                    <span>查詢</span>
                                </el-button>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
            </el-row>
            <el-row>
                <el-col>
                    <el-table :data="tableData" border style="width: 90%">
                        <el-table-column type="index" label="序號" align="center" ></el-table-column>
                        <el-table-column prop="name" label="用戶名" align="center" ></el-table-column>
                        <el-table-column prop="phone" label="手機號" align="center" ></el-table-column>
                        <el-table-column prop="email" label="電郵" align="center" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="title" label="標題" align="center" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="type" label="類型" align="center" >
                            <template slot-scope="scope">
                                <span v-if="scope.row.type == 1">優惠報告</span>
                                <span v-if="scope.row.type == 2">信用卡申請領取積分</span>
                            </template>
                        </el-table-column>
                        <el-table-column prop="content" label="內容" align="center" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="createTime" label="創建時間" align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.createTime | dateFmt('yyyy-MM-dd')}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column label="操作" width="300" fixed="right" align="center">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="toDelete(scope.row)" v-text="scope.row.deleted == 'N' ? '刪除' : '恢復'"></el-button>
                                <el-button type="text" size="small" @click="toDetail(scope.row)" >詳情</el-button>
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
      brs: [{ name: "報告管理", to: { name: "reportManager" } }],
      total: 100,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        key: "",
      },
      tableData: [],
    };
  },
  mounted() {
    this.getTableData();
  },
  computed: {
    
  },
  methods: {
    toDetail(row){
      this.$to({ name: "reportDetail", query: {id:row.id} });
    },
    toDelete(row){
      let visibtext = null;
      let deleted = null;
      if (row.deleted == "N") {
        visibtext = "刪除";
        deleted = "Y";
      }
      if (row.deleted == "Y") {
        visibtext = "恢復";
        deleted = "N";
      }
      this.$confirm("是否確認對該報告進行" + visibtext + "操作", "提示", {
        confirmButtonText: "確定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
          this.$post("reportSave", { id: row.id, deleted: deleted }).then(res => {
            this.getTableData();
            this.$message({ message: "操作成功", 
                            type: "success",
                            center:true });
          });
    }).catch(() => {});
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("reportList", this.queryMap)
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