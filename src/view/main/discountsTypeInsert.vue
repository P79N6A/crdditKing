<template>
    <ui-layout :brs="brs">
        <ui-container>
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="130px">
                    <el-col >
                      <el-row :span="12">
                            <el-form-item label="icon" prop="icon">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccessIcon"
                                    >
                                    <img v-if="formData.icon" :src="formData.icon" class="avatarIcon">
                                    <i v-else class="el-icon-plus avatar-uploader-icon-icon"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="圖標" prop="url">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccessUrl"
                                    >
                                    <img v-if="formData.url" :src="formData.url" class="avatarUrl">
                                    <i v-else class="el-icon-plus avatar-uploader-icon-url"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                        <!-- <el-row :span="12">
                            <el-form-item label="列表banner" prop="banner">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccessBanner"
                                    >
                                    <img v-if="formData.banner" :src="formData.banner" class="avatar">
                                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row> -->
                        <el-row :span="12">
                            <el-form-item label="名稱">
                                <el-input v-model="formData.name" style="width:200px;"></el-input>
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
      detailOption: {},
      brs: [{ name: "新增精選特惠", to: { name: "discountsTypeInsert" } }],
      formData: {
        url:null,
        name:null,
        icon:null,
        banner:null
      },
      rules: {},
    };
  },
  components: {
    
  },
  computed: {
   
  },
  mounted() {
  },
  methods: {
    handleAvatarSuccessUrl(res, file) {
      this.formData.url = URL.createObjectURL(file.raw);
      this.formData.url = res.data;
    },
    handleAvatarSuccessBanner(res, file) {
      this.formData.banner = URL.createObjectURL(file.raw);
      this.formData.banner = res.data;
    },
    handleAvatarSuccessIcon(res, file) {
      this.formData.icon = URL.createObjectURL(file.raw);
      this.formData.icon = res.data;
    },
    toEdit() {
      this.$refs["formData"].validate(valid => {
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("discountsTypeSave", this.formData)
            .then(res => {
              this.$message({
                showClose: true,
                message: "新增精選特惠成功",
                type: "success",
                center:true
              });
              this.$to({ name: "discountsTypeManager" });
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
.avatar-uploader-icon-icon {
  font-size: 28px;
  color: #8c939d;
  width: 100px;
  height: 100px;
  line-height: 100px;
  text-align: center;
}
.avatar-uploader-icon-url {
  font-size: 28px;
  color: #8c939d;
  width: 100px;
  height: 100px;
  line-height: 100px;
  text-align: center;
}
.avatar {
  width: 300px;
  height: 178px;
  display: block;
}
.avatarIcon {
  width: 100px;
  height: 100px;
  display: block;
}
.avatarUrl {
  width: 100px;
  height: 100px;
  display: block;
}
</style>