<template>
    <ui-layout :brs="brs">
        <ui-container>
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="130px">
                    <el-col >
                      <el-row :span="12">
                            <el-form-item label="圖片" prop="url">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccess"
                                    >
                                    <img v-if="formData.url" :src="formData.url" class="avatar">
                                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="名稱">
                                <el-input v-model="formData.name" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                         <el-row :span="12">
                             <el-form-item label="優惠日期">
                                     <el-date-picker v-model="discountsTimeNull" @change="getSearchTime" placeholder="選擇優惠日期" style="width:200px;">
                                         <template slot="prepend">優惠日期</template>
                                     </el-date-picker>
                             </el-form-item>
                        </el-row>
                        <el-row :span="12" >
                            <el-form-item label="簡介">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.detail" ref="myQuillEditor" class="editer" :options="detailOption" @ready="detailReady($event)" style="height:300px;">
                                    </quill-editor>
                                </div>
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
import { quillEditor } from "vue-quill-editor"; //调用编辑器
import UE from '../../components/editor.vue';
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
export default {
  components: { Loading,UE },
  data() {
    return {
      detailOption: {},
      brs: [{ name: "新增信用卡優惠", to: { name: "discountsChoiceInsert" } }],
      formData: {
        url:null,
        name:null,
        detail:null,
        discountsTime:null
      },
      rules: {},
      discountsTimeNull:null
    };
  },
  components: {
    //使用编辑器
    quillEditor,
    UE
  },
  computed: {
    editor() {
      return this.$refs.myQuillEditor.quill;
    }
  },
  mounted() {
  },
  methods: {
    //时间搜索
    getSearchTime() {
      this.formData.discountsTime = this.$getYMD(this.discountsTimeNull);
    },  
    //富文本 簡介
    detailReady(editor) {
    },
    handleAvatarSuccess(res, file) {
      this.formData.url = URL.createObjectURL(file.raw);
      this.formData.url = res.data;
    },
    toEdit() {
      this.$refs["formData"].validate(valid => {
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("discountsChoiceSave", this.formData)
            .then(res => {
              this.$message({
                showClose: true,
                message: "新增信用卡類型成功",
                type: "success",
                center:true
              });
              this.$to({ name: "discountsChoiceManager" });
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