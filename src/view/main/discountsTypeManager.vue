<template>
    <ui-layout :brs="brs">
        <ui-container style="height:50rem;overflow-y:scroll">
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-input v-model="queryMap.name" placeholder="請輸入精選特惠名稱" clearable></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label=" ">
                                <el-button type="primary" @click="getTableData">
                                    <i class="el-icon-search"></i>
                                    <span>查詢</span>
                                </el-button>
                                <el-button type="primary" @click="discountsTypeInsert">
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
                        <el-table-column prop="icon" label="icon"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.icon" :src="scope.row.icon" style="width:10px;height:10px;">
                          </template>
                        </el-table-column>
                        <el-table-column prop="url" label="圖標"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.url" :src="scope.row.url" style="width:50px;height:50px;">
                          </template>
                        </el-table-column>
                        <!-- <el-table-column prop="banner" label="列表banner"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.banner" :src="scope.row.banner" style="width:150px;height:50px;">
                          </template>
                        </el-table-column> -->
                        <el-table-column prop="name" label="優惠類別名稱" align="center" ></el-table-column>
                        <el-table-column prop="createTime" label="創建時間" align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.createTime | dateFmt('yyyy-MM-dd')}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column label="操作" width="300" fixed="right" align="center">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="toDelete(scope.row)" v-text="scope.row.deleted == 'N' ? '刪除' : '已刪除'"></el-button>
                                <el-button type="text" size="small" @click="discountsTypeDetail(scope.row)" style="margin-left:10px;">编辑</el-button>
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
      brs: [{ name: "精選特惠", to: { name: "discountsTypeManager" } }],
      total: 100,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        name: "",
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
     //跳转新增页
    discountsTypeInsert() {
      this.$to({ name: "discountsTypeInsert" });
    },
    //编辑
    discountsTypeDetail(row){
        this.$to({ name: "discountsTypeDetail", query: {id:row.id} });
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
      this.$confirm("是否確認對該精選特惠進行" + visibtext + "操作", "提示", {
        confirmButtonText: "確定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
          this.$post("discountsTypeSave", { id: row.id, deleted: deleted }).then(res => {
            this.getTableData();
            this.$message({ message: "操作成功", 
                            type: "success",
                            center:true });
          });
    }).catch(() => {});
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("discountsTypeList", this.queryMap)
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