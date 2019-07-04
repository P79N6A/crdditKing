<template>
    <ui-layout :brs="brs">
        <ui-container>
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="180px">
                    <el-col >
                        <el-row :span="12">
                            <el-form-item label="所屬電子貨幣包">
                                 <el-select filterable v-model="formData.eMoneyId" placeholder="請選擇電子貨幣包" @change="eMoneyChange" size="18">
                                    <el-option v-for="i in eMoneyArr" :key="i.name" :label="i.name" :value="i.id"></el-option>
                                </el-select>
                            </el-form-item>
                        </el-row>
                       <el-row :span="12">
                            <el-form-item label="電子貨幣信用卡名稱">
                                <el-input v-model="formData.name" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="回贈里數">
                                <el-input v-model="formData.rebateMileage" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="最高回贈(每年/每月)">
                                <el-input v-model="formData.tallRebate" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row>        
                            <el-form-item label=" ">
                                <div style="margin-left:50px;margin-top:100px;">
                                    <el-button type="primary" @click="toEdit">確認新增</el-button>
                                    <el-button type="primary" @click="$back()">關閉</el-button>
                                </div>
                            </el-form-item>
                        </el-row>
                    </el-col>
                </el-form>
           
        </ui-container>
    </ui-layout>
</template>

<script>
import { Loading } from "element-ui";
export default {
  components: { Loading },
  data() {
    return {
      brs: [{ name: "添加電子貨幣信用卡", to: { name: "eCardInsert" } }],
      formData: {
        eMoneyId:null,
        rebateMileage:null,
        name:null,
        tallRebate:null
      },
      eMoneyArr:[],
      queryMap: {
        pageNo: 1,
        pageSize: 10000,
      },
    };
  },
  computed: {
   
  },
  mounted() {
    this.geteMoneyList();
  },
  methods: {
    //下拉选择信用卡类型
    eMoneyChange(val){
      this.formData.eMoneyId = val;
    },
    //電子貨幣包集合
    geteMoneyList(){
         let loadingInstance = Loading.service({ fullscreen: true });
         this.$post("eMoneyList", this.queryMap)
        .then(res => {
            this.eMoneyArr = res.list;
            loadingInstance.close();
        }).catch(() => {
            loadingInstance.close();
        });
    },
    toEdit() {
      this.$refs["formData"].validate(valid => {
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("eCardSave", this.formData)
            .then(res => {
              this.$message({
                showClose: true,
                message: "新增電子貨幣信用卡成功",
                type: "success",
                center:true
              });
              this.$to({ name: "ecardManager" });
              loadingInstance.close();
            })
            .catch(() => {
              loadingInstance.close();
            });
        } else {
          return false;
        }
      });
    }
  }
};
</script>

<style lang="less">
@import "../../style/teaend.less";
.imgcontainer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  > div {
    text-align: center;
    > p {
      margin-bottom: 20px;
    }
    > div {
      border: 1px solid #999;
      padding: 10px;
      width: 300px;
      height: 300px;
      > img {
        max-width: 280px;
        max-height: 280px;
      }
    }
  }
}
.margintop20 {
  margin-top: 20px;
}
.tenzi {
  content: "";
  display: block;
  border: 0;
  background-color: #9b9b9b;
  border-radius: 5px;
  position: absolute;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 300px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 300px;
  height: 178px;
  display: block;
}
</style>