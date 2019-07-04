<template>
    <ui-layout :brs="brs">
        <ui-container style="height:50rem;overflow-y:scroll">
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="">
                                <el-input v-model="queryMap.keyword" placeholder="請輸入信用卡名稱" clearable></el-input>
                            </el-form-item>
                             <el-form-item >
                               <el-select v-model="queryMap.bankId" placeholder="全部銀行" style="width:200px;" @change="getTableData">
                                   <el-option label="全部銀行" value=""></el-option>
                                    <el-option
                                    v-for="item in bankList"
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
                                <el-button type="primary" @click="cardInsert">
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
                        <el-table-column prop="imgUrl" label="信用卡圖片"  align="center" width="180px;">
                          <template slot-scope="scope">
                              <img v-if="scope.row.imgUrl" :src="scope.row.imgUrl" style="width:150px;height:50px;">
                          </template>
                        </el-table-column>
                        <el-table-column prop="cardName" label="信用卡名稱" align="center" width="150px;"  :show-overflow-tooltip="true"></el-table-column>
                       <el-table-column prop="award" label="迎新獎賞" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                       <el-table-column prop="content" label="迎新內容" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="discounts" label="信用卡優惠" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="discountsContent" label="信用卡優惠內容" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="onlyTitle" label="獨家優惠標題" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column prop="onlyContent" label="獨家優惠內容" align="center" width="150px;" :show-overflow-tooltip="true"></el-table-column>
                        <el-table-column label="操作" width="200px;" fixed="right" align="center">
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
      brs: [{ name: "信用卡迎新優惠", to: { name: "cardDiscountsManager" } }],
      total: 100,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
        keyword: "",
        bankId:null
      },
      tableData: [],
      //銀行
      bankList:[],
      //分頁
      queryPageMap:{
        pageNo:1,
        pageSize:10000
      }
    };
  },
  mounted() {
    this.getTableData();
    this.getBankList();
  },
  computed: {
    
  },
  methods: {
    getBankList() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("bankList",this.queryPageMap)
        .then(res => {
          this.bankList = res.list;
          loadingInstance.close();
        })
        .catch(() => {
          loadingInstance.close();
        });
    },
    //跳转新增页
    cardInsert() {
      this.$to({ name: "cardDiscountsNewInsert" });
    },
    //编辑
    toEdit(row){
        this.$to({ name: "cardDiscountsNewDetail", query: {id:row.id} });
    },
    toDelete(row){
      let visibtext = null;
      let deleted = null;
      if (row.deleted == "N") {
        visibtext = "刪除";
        deleted = "Y";
      }
      if (row.deleted == "Y") {
        visibtext = "回復";
        deleted = "N";
      }
      this.$confirm("是否確認對該信用卡迎新優惠進行" + visibtext + "操作", "提示", {
        confirmButtonText: "確定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
          this.$post("cardDiscountsNewSave", { id: row.id, deleted: deleted }).then(res => {
            this.getTableData();
            this.$message({ message: "操作成功", 
                            type: "success",
                            center:true });
          });
    }).catch(() => {});
    },
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("cardDiscountsNewList", this.queryMap)
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