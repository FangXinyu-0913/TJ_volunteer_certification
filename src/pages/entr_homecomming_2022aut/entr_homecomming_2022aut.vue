<template>
    <view class="page_field">
      <image
        class="image"
        mode="widthFix"
        src="https://636c-cloud1-7gs40qcu242746b1-1311667620.tcb.qcloud.la/pic/red_hat_2022aut_background-2.jpg?sign=7d50f7ff031e8f227a0a78c75f7cc225&t=1663677288"
      ></image>
      <view class="input_card">
        <view class="title"> 信息填写 </view>
        <AtInput
          :border="false"
          title="姓名"
          type="text"
          placeholder="请输入姓名"
          v-model:value="this.name"
          :adjustPosition="true"
        />
        <AtInput
          :border="false"
          title="学号"
          type="text"
          placeholder="请输入您的学号"
          v-model:value="this.student_id"
          :adjustPosition="true"
        />
        <view class="mine_button">
          <AtButton type="primary" @click="submit" :loading="this.button_loading"
            >提交</AtButton
          >
        </view>
      </view>
      <AtToast :isOpened="this.show_toast" :text="text" status="error"></AtToast>
    </view>
  </template>
  
  <script>
  import "./entr_homecomming_2022aut.css";
  import { AtInput, AtButton } from "taro-ui-vue3";
  import "taro-ui-vue3/dist/style/components/input.scss";
  import "taro-ui-vue3/dist/style/components/button.scss";
  import Taro from "@tarojs/taro";
  export default {
    components: {
      AtInput,
      AtButton,
    },
    data() {
      return {
        name: "",
        student_id: "",
        button_loading: false,
        show_toast: false,
      };
    },
    methods: {
      submit() {
        if (this.name == "" || this.student_id == "") {
          Taro.showToast({
            // title: "未找到相关信息,请提交志愿服务证明至tj_vs@163.com进行补录,邮件标题为'疫情服务证书补录-姓名'",
            title: "请将信息填写完整",
            icon: "error",
            duration: 3000,
          });
          return;
        }
        else{
        this.button_loading = true;
        Taro.cloud
          .callFunction({
            name: "getStudentInfor",
            data: {
              name: this.name,
              dbname:'redhat22fall',
              student_id: this.student_id,
            },
          })
          .then((res) => {
            this.button_loading = false;
            if (res.result) {
              Taro.navigateTo({
                url:
                  "../certi_homecomming_2022aut/certi_homecomming_2022aut?name=" +
                  encodeURIComponent(JSON.stringify(this.name))+"&student_id="+encodeURIComponent(JSON.stringify(this.student_id)),
              });
            } else {
              Taro.showToast({
                // title: "未找到相关信息,请提交志愿服务证明至tj_vs@163.com进行补录,邮件标题为'疫情服务证书补录-姓名'",
                title: "未找到相关信息,补录方式请查看推送",
                icon: "none",
                duration: 3000,
              });
              this.name="";
              this.student_id="";
            }
          })
          .catch((errMsg) => {
            this.button_loading = false;
            console.log(errMsg);
          });
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .at-input__title {
    font-size: 34px;
    width: 34px;
    color: #3c3c3c;
  }
  </style>
  