<template>
    <ui-layout :brs="brs">
        <ui-container>
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="100px">
                    <el-col >
                       
                        <el-row :span="12">
                            <el-form-item label="標題">
                                <el-input v-model="formData.title"  :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>

                        <el-row :span="12">
                            <el-form-item label="用戶名">
                                <el-input v-model="formData.name"  :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>

                        <el-row :span="12">
                            <el-form-item label="手機號">
                                <el-input v-model="formData.phone"  :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>

                        <el-row :span="12">
                            <el-form-item label="電郵">
                                <el-input v-model="formData.email"  :disabled="true"></el-input>
                            </el-form-item>
                        </el-row>

                        <el-row :span="12">
                            <el-form-item label="內容">
                                <el-input v-model="formData.content"  :disabled="true" type="textarea" :rows="10" style="width:900px;"></el-input>
                            </el-form-item>
                        </el-row>
                  
                        <el-row :span="12">
                            <el-form-item label="報告圖片">
                                <div style="width: 800px">
                                    <el-upload
                                    action=""
                                    list-type="picture-card"
                                    :file-list="viewGoodsDetailImgList"
                                    multiple
                                    :disabled=true>
                                    </el-upload>
                                </div>
                            </el-form-item>
                        </el-row>
                        <el-row>
                            <el-form-item label=" ">
                                <div style="margin-left:50px;margin-top:100px;">
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
import { viewImg } from "../../../config/project/project.env";
import { Loading } from "element-ui";
export default {
  components: { Loading },
  data() {
    return {
      brs: [{ name: "報告詳情", to: { name: "reportDetail" } }],
      formData: {},
      viewGoodsDetailImgList:[],
      //图片预览地址
      viewImg:viewImg,
      dialogDetailVisible:false
    };
  },
 
  computed: {

  },
  created() {
    let that = this;
    this.$get("reportDetail", this.$route.query.id)
      .then(res => {
        this.formData = res;
        if(res.imgs.length>0){
            that.viewGoodsDetailImgList = res.imgs.map((i)=>{
                let item = {
                    name: i,
                    url: i
                }
                return item;
            })
        }
       
        loadingInstance.close();
      })
      .catch(() => {
      });
  },
  methods: {
   
    
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
.avatars {
  width: 100px;
  height: 100px;
  display: block;
}
</style>