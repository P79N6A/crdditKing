<template>
    <ui-layout :brs="brs">
        <ui-container style="height:50rem;overflow-y:scroll">
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="7">
                            <el-form-item label="">
                                <el-input v-model="queryMap.keyword" placeholder="請輸入商戶名稱" clearable></el-input>
                            </el-form-item>
                             <el-form-item >
                               <el-select v-model="queryMap.discountsTypeId" placeholder="全部分类" style="width:200px;" @change="getTableData">
                                   <el-option label="全部分类" value=""></el-option>
                                    <el-option
                                    v-for="item in discountsTypeList"
                                    :key="item.name"
                                    :label="item.name"
                                    :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label=" ">
                                <el-button type="primary" @click="getTableData">
                                    <i class="el-icon-search"></i>
                                    <span>查詢</span>
                                </el-button>
                                <el-button type="primary" @click="merchantInsert">
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
                        <el-table-column prop="img" label="商戶圖片"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.img" :src="scope.row.img" style="width:50px;height:50px;">
                          </template>
                        </el-table-column>
                        <el-table-column prop="name" label="商戶名稱" align="center" ></el-table-column>
                        <el-table-column prop="discountsTypeName" label="優惠類別名稱" align="center" ></el-table-column>
                        <el-table-column prop="createdTime" label="創建時間" align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.createdTime | dateFmt('yyyy-MM-dd')}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column label="操作" width="300" fixed="right" align="center">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="toDelete(scope.row)" v-text="scope.row.deleted == 'N' ? '刪除' : '已刪除'"></el-button>
                                <el-button type="text" size="small" @click="toEdit(scope.row)" style="margin-left:10px;">編輯</el-button>
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
      brs: [{ name: "商戶管理", to: { name: "merchantManager" } }],
      total: 100,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        keyword: "",
        discountsTypeId:null
      },
      tableData: [],
      queryDiscountsMap:{
        pageNo:1,
        pageSize:10000
      },
      //優惠類別
      discountsTypeList:[],
      //刪除用
      merchant:{
        id:null,
        deleted:null
      }
    };
  },
  mounted() {
    this.getTableData();
    this.getDiscountsTypeList();
  },
  computed: {
    
  },
  methods: {
    getDiscountsTypeList() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("discountsTypeList",this.queryDiscountsMap)
        .then(res => {
          this.discountsTypeList = res.list;
          loadingInstance.close();
        })
        .catch(() => {
          loadingInstance.close();
        });
    },
     //跳转新增页
    merchantInsert() {
      this.$to({ name: "merchantInsert" });
    },
    //编辑
    toEdit(row){
        this.$to({ name: "merchantDetail", query: {id:row.id} });
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
      this.$confirm("是否確認對該商戶進行" + visibtext + "操作", "提示", {
        confirmButtonText: "確定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.merchant.id = row.id;
        this.merchant.deleted = row.deleted;
          this.$post("merchantSave", {merchant:this.merchant}).then(res => {
            this.getTableData();
            this.$message({ message: "操作成功", 
                            type: "success",
                            center:true });
          });
    }).catch(() => {});
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("merchantList", this.queryMap)
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