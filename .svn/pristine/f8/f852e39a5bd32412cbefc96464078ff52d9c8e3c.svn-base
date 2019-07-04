<template>
    <ui-layout :brs="brs">
        <ui-container>
          <!-- <el-row>
                <el-form :inline="true" class="demo-form-inline">
                    <el-row >
                            <el-form-item label=" ">
                                <el-button type="primary" @click="toInsert">
                                    <i class="el-icon-circle-plus-outline"></i>
                                    <span>添加</span>
                                </el-button>
                            </el-form-item>
                    </el-row>
                </el-form>
            </el-row> -->
            <el-row>
                <el-table :data="tableData" border style="max-width:80%;">
                  <!-- <el-table-column prop="phone" label="客服手机号" align="center"></el-table-column> -->
                  <!-- <el-table-column prop="qq" label="客服QQ" align="center"></el-table-column> -->
                  <el-table-column prop="img" label="客服二维码"  align="center">
                      <template slot-scope="scope">
                        <img v-if="scope.row.img" :src="scope.row.img" style="width:40px;height:40px;">
                    </template>
                  </el-table-column>
                    <!-- <el-table-column prop="wx" label="微信"  align="center"></el-table-column>
                    <el-table-column prop="tel" label="电话"  align="center"></el-table-column> -->
                  <el-table-column label="操作" fixed="right" align="center" width="300px;">
                      <template slot-scope="scope"> 
                          <el-button type="text" size="small" @click="toEdit(scope.row)">修改</el-button>
                          <!-- <el-button type="text" size="small" @click="toDeleted(scope.row)">删除</el-button> -->
                      </template>
                  </el-table-column>
                </el-table>
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
export default {
  components: { UiPage, Loading },
  data() {
    return {
      brs: [{ name: "客服管理", to: { name: "configManager" } }],
      tableData: [],
      total:null,
      queryMap: {
        pageNo: 1,
        pageSize: 10,
      },
    };
  },
  mounted() {
    this.getTableData();
  },
  methods: {
    toDeleted(row) {
      this.$confirm("是否确认删除", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$post("customerServiceSave", { id: row.id, deleted: "Y" }).then(res => {
            this.getTableData();
            this.$message({ message: "删除成功", type: "success",center:true });
          });
        })
        .catch(() => {});
    },
    
    getTableData() {
      let loadingInstance = Loading.service({ fullscreen: true });
      this.$post("configList",this.queryMap)
        .then(res => {
          this.tableData = res.list;
          this.total = res.total;
          loadingInstance.close();
        })
        .catch(() => {
          loadingInstance.close();
        });
    },
    toEdit(row) {
      this.$to({ name: "configEdit", query: row });
    },
    toInsert(row) {
      this.$to({ name: "configInsert"});
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