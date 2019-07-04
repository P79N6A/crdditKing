<template>
    <ui-layout :brs="brs">
        <ui-container>
            <el-row>
                <el-form :inline="true" :model="formData" :rules="rules" ref="formData" class="demo-form-inline" label-position="center" label-width="100px">
                    <el-col>
                        <el-row :span="12">
                            <el-form-item label="昵称" prop="nickName">
                                <el-input v-model="formData.nickName" :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="姓名" prop="name">
                                <el-input v-model="formData.name" :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="身份证" prop="idCard">
                                <el-input v-model="formData.idCard" :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="银行卡" prop="bankNumber">
                                <el-input v-model="formData.bankNumber" :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="期数" prop="issue">
                                <el-input v-model="formData.issue" :disabled="true">
                                  <template slot="append">期</template>
                                </el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="每期利息" prop="interest">
                                <el-input v-model="formData.interest" :disabled="true">
                                  <template slot="append">元</template>
                                </el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="支付宝账号" prop="alipayAccount">
                                <el-input v-model="formData.alipayAccount" :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="服务金额" prop="serviceMoney">
                                <el-input v-model="formData.serviceMoney" :disabled="true">
                                  <template slot="append">元</template>
                                </el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="金额" prop="money">
                                <el-input v-model.number="formData.money" autocomplete="off" type="number">
                                  <template slot="append">元</template>
                                </el-input>
                            </el-form-item>
                        </el-row>
                        
                        
                    <el-row>
                        <el-form-item label=" ">
                            <div class="margin20">
                                <el-button type="primary" @click="toSave">确认修改</el-button>
                                <el-button type="primary" @click="$back()">关闭</el-button>
                            </div>
                        </el-form-item>
                    </el-row>
                    </el-col>
                </el-form>
            </el-row>
        </ui-container>
    </ui-layout>
</template>

<script>
import Citys from "../../utils/citys.js";
import { viewImg } from "../../../config/project/project.env"; //图片预览
import { Loading } from "element-ui";
export default {
  components: { Loading },
  data() {
    return {
      brs: [{ name: "编辑贷款", to: { name: "loansEdit" } }],
      formData: {
        nickName: 0,
        name: null,
        idCard: null,
        bankNumber: null,
        issue: null,
        interest: null,
        alipayAccount:null,
        serviceMoney:null,
        money:null
      },
      rules: {},
    };
  },
  mounted() {
      this.getLoans();
  },
  methods: {
      getLoans(){
            let loadingInstance = Loading.service({fullscreen: true});
            this.$get('loansDetail',this.$route.query.id).then((res)=>{
                this.formData = res;
                loadingInstance.close();
            }).catch(()=>{loadingInstance.close();})
        },
    toSave() {
      this.$refs["formData"].validate(valid => {
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("loansSave", this.formData)
            .then(res => {
              this.$message({ message: "编辑贷款成功", type: "success" ,center:true});
              loadingInstance.close();
              this.$to({ name: "loansManager"});
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
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>