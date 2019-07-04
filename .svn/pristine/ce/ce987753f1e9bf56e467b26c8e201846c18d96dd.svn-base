<template>
    <ui-layout :brs="brs">
        <ui-container>
            <el-row>
                <el-form :inline="true" :model="queryMap" class="demo-form-inline" label-width="100px">
                    <el-row>
                        <el-col :span="3">
                            <el-form-item label="">
                                <el-input v-model="queryMap.key" placeholder="請輸入用戶名" clearable></el-input>
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
                    <el-table :data="tableData" border style="width: 80%">
                        <el-table-column type="index" label="序號" align="center" ></el-table-column>
                        <el-table-column prop="memberName" label="用戶名" align="center" ></el-table-column>
                        <el-table-column prop="goodsName" label="禮物名稱" align="center" ></el-table-column>
                        <el-table-column prop="name" label="收貨人" align="center" ></el-table-column>
                        <el-table-column prop="phone" label="手機號" align="center" ></el-table-column>
                        <el-table-column prop="area" label="地區" align="center" ></el-table-column>
                        <el-table-column prop="address" label="詳細地址" align="center" ></el-table-column>
                        <el-table-column prop="createTime" label="兌換時間" align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.createTime | dateFmt('yyyy-MM-dd')}}</span>
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
      brs: [{ name: "兌換記錄", to: { name: "exchangeRecordManager" } }],
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
 
  methods: {
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("exchangeRecordList", this.queryMap)
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