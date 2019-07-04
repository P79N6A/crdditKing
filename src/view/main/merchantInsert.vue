<template>
    <ui-layout :brs="brs">
        <ui-container style="height:50rem;overflow-y:scroll">
                <el-form :inline="true" :model="formData" ref="formData" class="demo-form-inline" label-position="center" label-width="150px">
                    <el-col >
                      <el-row :span="12">
                            <el-form-item label="商戶圖片" prop="img">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarSuccess"
                                    >
                                    <!-- action="http://192.168.31.123:8090/ck/file/upload" -->
                                    
                                    <img v-if="formData.merchant" :src="formData.merchant.img" class="avatar"/>
                                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                            <el-form-item label="商戶Logo" prop="logo">
                                <el-upload
                                    class="avatar-uploader"
                                    action="http://47.75.102.93:80/ck/file/upload"
                                    :show-file-list="false"
                                    :on-success="handleAvatarLogoSuccess"
                                    >
                                    
                                    <img v-if="formData.merchant" :src="formData.merchant.logo" class="avatar-logo"/>
                                    <i v-else class="el-icon-plus avatar-uploader-icon-logo"></i>
                                    </el-upload>
                            </el-form-item>
                        </el-row>
                       <el-row :span="12">
                            <el-form-item label="商戶名稱">
                                <el-input v-model="formData.merchant.name" style="width:200px;" ></el-input>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                          <el-form-item label="優惠類別">
                               <el-select filterable v-model="formData.merchant.discountsTypeId" placeholder="請選擇" @change="discountsTypeChange" style="width:200px;">
                                    <el-option
                                    v-for="item in discountsTypeList"
                                    :key="item.name"
                                    :label="item.name"
                                    :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                        </el-row>
                        <el-row :span="12">
                                <el-form-item label="銀行" >
                                    <el-row v-for="(a,i) in formData.merchantBank" :key="i" style="margin-bottom:10px;" >
                                        <el-select filterable v-model="formData.merchantBank[i]['bank']" placeholder="请选择" @change="goodsBankChange" style="width:130px;">
                                            <el-option v-for="i in bankArr" :key="i.name" :label="i.name" :value="i.id"></el-option>
                                        </el-select>
                                        <el-date-picker v-model="formData.merchantBank[i]['saleTime']" type="date" @change="getSearchTime(i)" placeholder="優惠開始時間" style="width:130px;" ></el-date-picker>
                                        <el-date-picker v-model="formData.merchantBank[i]['saleTimeEnd']" type="date" @change="getSearchTimeEnd(i)" placeholder="優惠截止時間" style="width:130px;" ></el-date-picker>
                                        <el-select v-model="formData.merchantBank[i]['deadline']" clearable placeholder="優惠期限" style="width:130px;">
                                          <el-option
                                            v-for="item in options"
                                            :key="item.value"
                                            :label="item.label"
                                            :value="item.value">
                                          </el-option>
                                        </el-select>
                                         <el-input placeholder="優惠簡介" v-model="formData.merchantBank[i]['intro']" style="width:150px;" type="textarea" :autosize="{ minRows: 1, maxRows: 3}" >
                                        </el-input>
                                        <el-input placeholder="優惠亮點"  v-model.number="formData.merchantBank[i]['lightspot']" style="width:150px;" type="textarea" :autosize="{ minRows: 1, maxRows: 3}" >
                                        </el-input>
                                        <el-input placeholder="優惠詳情" v-model="formData.merchantBank[i]['detail']" style="width:150px;" type="textarea" :autosize="{ minRows: 1, maxRows: 3}" >
                                        </el-input>
                                          <el-input placeholder="更多優惠詳情" v-model="formData.merchantBank[i]['details']" style="width:150px;margin-top:5px;" type="textarea" :autosize="{ minRows: 1, maxRows: 3}" >
                                          </el-input>
                                    </el-row>
                                </el-form-item>
                                <el-form-item label="" style="margin-top:5px;">
                                        <el-button type="primary" @click="addBank()" style="width:60px;" >添加</el-button>
                                </el-form-item>
                                <el-form-item label="" style="margin-top:5px;">
                                        <el-button type="primary" @click=" formData.merchantBank.splice(formData.merchantBank.length-1,1)" style="width:60px;" >删除</el-button>
                                </el-form-item>
                            </el-row>
                          <el-row :span="12">
                                <el-form-item label="門店" >
                                    <el-row v-for="(a,i) in formData.merchantShop" :key="i" style="margin-bottom:10px;">
                                         <el-input placeholder="請輸入門店名稱" v-model="formData.merchantShop[i]['name']" style="width:270px;" >
                                           <template slot="append">門店名稱</template>
                                        </el-input>
                                         <el-input placeholder="門店地址" v-model="formData.merchantShop[i]['address']" style="width:300px;"  >
                                            <template slot="append">地址</template>
                                        </el-input>
                                        <el-input placeholder="網絡地址" v-model="formData.merchantShop[i]['webLink']" style="width:300px;"  >
                                            <template slot="append">網址</template>
                                        </el-input>
                                        <el-input placeholder="聯繫電話"  v-model.number="formData.merchantShop[i]['telephone']" style="width:200px;" type="number"  >
                                            <template slot="append">電話</template>
                                        </el-input>
                                    </el-row>
                                </el-form-item>
                                <el-form-item label="">
                                        <el-button type="primary" @click="addShop()" style="width:60px;" >添加</el-button>
                                </el-form-item>
                                <el-form-item label="">
                                        <el-button type="primary" @click=" formData.merchantShop.splice(formData.merchantShop.length-1,1)" style="width:60px;" >删除</el-button>
                                </el-form-item>
                            </el-row>
                        <el-row>             
                            <el-form-item label=" ">
                                <div style="margin-left:50px;margin-top:100px;">
                                    <el-button type="primary" @click="toEdit">確認添加</el-button>
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
      brs: [{ name: "添加商戶", to: { name: "merchantInsert" } }],
       formData: {
       merchant:{
        img:null,
        logo:null,
        name:null,
        discountsTypeId:null,
       },
       merchantBank:[
         {
           bank:null,
           saleTime:null,
           saleTimeEnd:null,
           intro:null,
           lightspot:null,
           detail:null,
           details:null
         }
       ],
       merchantShop:[
         {
           name:null,
           address:null,
           telephone:null
         }
       ]
      },
      rules: {},
      //優惠類別
      discountsTypeList:[],
      queryDiscountsMap:{
        pageNo:1,
        pageSize:10000
      },
      //優惠開始時間
      saleTime:null,
      //優惠截止時間
      saleTimeEnd:null,
      bankArr:[],
       options: [{
          value: '今日限定',
          label: '今日限定'
        }, {
          value: '期間限定',
          label: '期間限定'
        }, {
          value: '全年優惠',
          label: '全年優惠'
        }],
    };
  },
  components: {
    
  },
  computed: {
    
  },
  mounted() {
      this.getDiscountsTypeList();
      this.getBankList();
  },
  methods: {
    //银行集合
    getBankList(){
         let loadingInstance = Loading.service({ fullscreen: true });
         this.$post("bankList", this.queryDiscountsMap)
        .then(res => {
            this.bankArr = res.list;
            loadingInstance.close();
        }).catch(() => {
            loadingInstance.close();
        });
    },
    //優惠開始日期
    getSearchTime(i) {
      console.log(i)
      this.formData.merchantBank[i].saleTime = this.$getYMD(this.formData.merchantBank[i]['saleTime']);
    },
    //優惠截止日期
    getSearchTimeEnd(i) {
      this.formData.merchantBank[i]['saleTimeEnd'] = this.$getYMD(this.formData.merchantBank[i]['saleTimeEnd']);
    },
    //下拉选择银行
    goodsBankChange(val) {
      this.formData.merchantBank[this.formData.merchantBank.length-1]['bank'] = val;
    },    
    //銀行
    addBank(blur) {
      // if (this.formData.merchantBank.length == 0) {
        this.formData.merchantBank.push({
          saleTime:null,
          saleTimeEnd:null,
          intro:"",
          lightspot:"",
          detail: "",
          bank: "",
          details:""
        });
      // } else if (!blur) {
      //   if (blur) return;
      //   this.formData.merchantBank.push({
      //     saleTime:null,
      //     saleTimeEnd:null,
      //     intro:"",
      //     lightspot:"",
      //     detail: "",
      //     bank: "",
      //     details:""
      //   });
      // } else if (this.formData.merchantBank.length > 0) {
      //   this.formData.merchantBank[0] = this.formData.merchantBank;
      // }
    },
    //門店
    addShop() {
      
        this.formData.merchantShop.push({
          name: "",
          address:"",
          telePhone:"",
        });
      
    },
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
    //優惠類別
    discountsTypeChange(val) {
      this.formData.discountsTypeId = val;
    },
    handleAvatarSuccess(res, file) {
      this.formData.merchant.img = URL.createObjectURL(file.raw);
      this.formData.merchant.img = res.data;
    },
    handleAvatarLogoSuccess(res, file) {
      this.formData.merchant.logo = URL.createObjectURL(file.raw);
      this.formData.merchant.logo = res.data;
    },
    toEdit() {
      this.$refs["formData"].validate(valid => {
        if (valid) {
          let loadingInstance = Loading.service({ fullscreen: true });
          this.$post("merchantSave", this.formData)
            .then(res => {
              this.$message({
                showClose: true,
                message: "編輯商戶成功",
                type: "success",
                center:true
              });
              this.$to({ name: "merchantManager" });
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
  width: 100px;
  height: 100px;
  line-height: 100px;
  text-align: center;
}

.avatar-uploader-icon-logo {
  font-size: 28px;
  color: #8c939d;
  width: 70px;
  height: 70px;
  line-height: 70px;
  text-align: center;
}
.avatar-logo {
  width: 70px;
  height: 70px;
  display: block;
}
.avatar {
  width: 100px;
  height: 100px;
  display: block;
}

</style>