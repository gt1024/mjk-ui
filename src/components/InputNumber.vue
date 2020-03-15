<template>
  <div>
    <input type="text" v-if="type == 0" @change="InputIdNum()" v-model.trim="id_num"
           :placeholder='$t("auth_new.b61c6aa0974f11e9a5048c8590a7c397")'
           maxlength="18" v-on:focus="id_show" v-on:blur="id_input">

    <input type="text" v-if="type == 1" pattern="[0-9]*" maxlength="11" @change="InputNum()" v-model="phoneNum"
           :placeholder='$t("inputnumber.b260ec9edb7811e9be9a88b2228b82a5")' v-on:focus="inputfocus"
           v-on:blur="inputblur">
  </div>
</template>
<script>
  import {Toast} from "mint-ui";

  export default {
    props: ["type", "val"],   //0 - 身份证  ， 1  - 手机号
    data() {
      return {
        temp: "",
        id_num: "",
        phoneNum: ""
      };
    },
//    mounted(){
//      setTimeout(() => {
//        this.getParams();
//      }, 3000);
//    },
    methods: {
      InputIdNum(){
        let str = this.id_num
        let reg = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
        if (reg.test(str)) {

          this.$emit("updatephone", this.id_num)
        } else {
          Toast(i18n.t("auth_new.b61c9750974f11e9a5048c8590a7c397"));
          this.id_num = ""
        }
      },

      InputNum(){
        let str = this.phoneNum
        let reg = /^[0-9]*$/;
        if (reg.test(str)) {

          this.$emit("updatephone", this.phoneNum)
        } else {
          Toast(this.$t("inputnumber.b5e8fbf2974f11e9a5048c8593a7c397"));
          this.phoneNum = ""
        }
      },

      inputfocus(){
        if(this.val){
          this.phoneNum = this.val;
        }else {
          this.phoneNum = this.temp;
        }

      },

      inputblur(){
        this.temp = this.phoneNum;
        this.phoneNum = this.code(this.phoneNum);
      },

      id_show(){
        this.id_num = this.temp;
      },

      id_input(){
        this.temp = this.id_num;
        this.id_num = this.code(this.id_num);
      },
      getParams(){
        this.phoneNum = this.$store.state.data;
      }
    },
//    computed: {
//      myValue() {
//          return this.$store.state.data
//      }
//    },
    watch: {
      val: function (newVal, oldVal) {
        //其他业务代码
        return newVal ? this.phoneNum = newVal : this.phoneNum = oldVal
      }
    }
  };
</script>
