<template>
    <ui-layout :brs="brs">
        <ui-container style="height:40rem;overflow-y:scroll">
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="130px">
                    <el-col >
                      <el-row :span="7">
                            <el-form-item label="信用卡圖片" prop="imgUrl">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccess"
                                    >
                                    
                                    <!-- action="http://192.168.31.123:8090/ck/file/upload" -->
                                    <img v-if="formData.imgUrl" :src="formData.imgUrl" class="avatar">
                                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                                    <!-- :before-upload="beforeAvatarUpload" -->
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                        <el-row :span="7">
                            <el-form-item label="名稱">
                                <el-input v-model="formData.name" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="7">
                            <el-form-item label="所属银行">
                                 <el-select filterable v-model="formData.bankId" placeholder="请选择" @change="bankChange" size="18">
                                    <el-option v-for="i in bankArr" :key="i.name" :label="i.name" :value="i.id"></el-option>
                                </el-select>
                            </el-form-item>
                        </el-row>
                        <el-row :span="20">
                                <el-form-item label="类别" >
                                    <el-row>
                                        <template>
                                          <el-checkbox v-model="formData.own" value=1>所有</el-checkbox>
                                        </template>
                                         <el-input placeholder="排序"  v-model.number="formData.ownSort" style="width:150px;margin-left:50px;" type="number">
                                            <template slot="append">排序</template>
                                        </el-input>
                                    </el-row>
                                    <el-row>
                                        <template>
                                          <el-checkbox v-model="formData.feedBack" value=1>現金回贈</el-checkbox>
                                        </template>
                                         <el-input placeholder="排序"  v-model.number="formData.feedBackSort" style="width:150px;margin-left:22px;margin-top:5px;" type="number" >
                                            <template slot="append">排序</template>
                                        </el-input>
                                    </el-row>
                                    <el-row>
                                        <template>
                                          <el-checkbox v-model="formData.mile" value=1>里數</el-checkbox>
                                        </template>
                                         <el-input placeholder="排序"  v-model.number="formData.mileSort" style="width:150px;margin-left:50px;margin-top:5px;" type="number" >
                                            <template slot="append">排序</template>
                                        </el-input>
                                    </el-row>
                                    <el-row>
                                        <template>
                                          <el-checkbox v-model="formData.gift" value=1>禮物</el-checkbox>
                                        </template>
                                         <el-input placeholder="排序"  v-model.number="formData.giftSort" style="width:150px;margin-left:50px;margin-top:5px;" type="number">
                                            <template slot="append">排序</template>
                                        </el-input>
                                    </el-row>
                                </el-form-item>   
                        </el-row>

                        <el-row :span="20">
                                <el-form-item label="精選內容" >
                                    <el-row>
                                        <template>
                                          <el-checkbox v-model="formData.choice" label="1">展示</el-checkbox>
                                        </template>
                                         <el-input placeholder="排序"  v-model.number="formData.choiceSort" style="width:150px;margin-left:50px;" type="number">
                                            <template slot="append">排序</template>
                                        </el-input>
                                    </el-row>
                                </el-form-item>   
                        </el-row>
                        
                       
                        <el-row :span="7">
                            <el-form-item label="迎新獎賞">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.award" ref="myQuillEditor" class="editer" :options="awardOption" @ready="awardReady($event)" style="height:250px;">
                                    </quill-editor>
                                </div>
                            </el-form-item>
                        </el-row>

                        
                        <el-row :span="7">
                            <el-form-item label="迎新內容" style="margin-top:50px;">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.content" ref="myQuillEditor" class="editer" :options="contentOpetion" @ready="contentReady($event)" style="height:250px;">
                                    </quill-editor>
                                </div>
                            </el-form-item>
                        </el-row>

                        
                        <el-row :span="7">
                            <el-form-item label="信用卡優惠" style="margin-top:50px;">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.discounts" ref="myQuillEditor" class="editer" :options="discountsOpetion" @ready="discountsReady($event)" style="height:250px;">
                                    </quill-editor>
                                </div>
                            </el-form-item>
                        </el-row>

                        <el-row :span="7">
                            <el-form-item label="信用卡優惠內容" style="margin-top:50px;">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.discountsContent" ref="myQuillEditor" class="editer" :options="discountsContentOpetion" @ready="discountsContentReady($event)" style="height:250px;">
                                    </quill-editor>
                                </div>
                            </el-form-item>
                        </el-row>

                        <el-row :span="7">
                            <el-form-item label="獨家優惠標題" style="margin-top:50px;">
                                <el-input v-model="formData.onlyTitle" style="width:200px;"></el-input>
                            </el-form-item>
                        </el-row>
                        
                        <el-row :span="7">
                            <el-form-item label="獨家優惠內容">
                                <div class="edit_container" >
                                    <quill-editor v-model="formData.onlyContent" ref="myQuillEditor" class="editer" :options="onlyContentOpetion" @ready="onlyContentReady($event)" style="height:250px;">
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
      awardOption:{}, //迎新獎賞
      contentOpetion:{}, //迎新內容
      discountsOpetion:{}, //信用卡優惠
      discountsContentOpetion:{}, //信用卡優惠內容
      onlyContentOpetion:{}, //獨家優惠內容
      brs: [{ name: "新增信用卡迎新優惠", to: { name: "cardDiscountsNewInsert" } }],
      formData: {
        name:null,
        imgUrl:null,
        bankId: null,
        own:null,
        ownSort:null,
        feedBack:null,
        feedBackSort:null,
        mile:null,
        mileSort:null,
        gift:null,
        giftSort:null,
        choice:null,
        choiceSort:null,
        award:null,
        content:null,
        discounts:null,
        discountsContent:null,
        onlyTitle:null,
        onlyContent:null,
      },
      //银行类型
      bankArr:[],
      rules: {},
      queryMap: {
        pageNo: 1,
        pageSize: 10000,
      },
     
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
      this.getBankList();
  },
  methods: {
    //下拉选择银行
    bankChange(val) {
      this.formData.bankId = val;
    },  
    //银行集合
    getBankList(){
         let loadingInstance = Loading.service({ fullscreen: true });
         this.$post("bankList", this.queryMap)
        .then(res => {
            this.bankArr = res.list;
            loadingInstance.close();
        }).catch(() => {
            loadingInstance.close();
        });
    },
    //富文本 迎新獎賞
    awardReady(editor){
    },
    //富文本 迎新內容
    contentReady(editor){
    },
    //富文本 信用卡優惠
    discountsReady(editor){
    },
    //富文本 信用卡優惠內容
    discountsContentReady(editor){
    },
    //富文本 獨家優惠內容
    onlyContentReady(editor){
    },
    handleAvatarSuccess(res, file) {
      this.formData.imgUrl = URL.createObjectURL(file.raw);
      this.formData.imgUrl = res.data;
    },
    toEdit() {
      this.$refs["formData"].validate(valid => {
        if(this.formData.own){
          
          this.formData.own = 1
          console.log(this.formData.own)
        }else{
          this.formData.own = 0
        }
        if(this.formData.feedBack){
          this.formData.feedBack = 1
        }else{
          this.formData.feedBack = 0
        }
        if(this.formData.mile){
          this.formData.mile = 1
        }else{
          this.formData.mile = 0
        }
        if(this.formData.gift){
          this.formData.gift = 1
        }else{
          this.formData.gift = 0
        }
        if(this.formData.choice){
          this.formData.choice = 1
        }else{
          this.formData.choice = 0
        }
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("cardDiscountsNewSave", this.formData)
            .then(res => {
              this.$message({
                showClose: true,
                message: "新增信用卡迎新優惠成功",
                type: "success",
                center:true
              });
              this.$to({ name: "cardDiscountsNewManager" });
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
.el-checkbox+.el-checkbox {
    margin-left: 0px;
}
</style>