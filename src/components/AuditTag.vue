<style>

</style>
<template>

  <van-tag :color="this.getColor(status)" size="medium">{{this.genStatusDesc(status)}}</van-tag>

</template>
<script>
  import {Tag} from "vant";
  export default {
    props: ["status"],
    data() {
      return {};
    },
    methods: {
      getColor(type) {
        let color;
        switch (type) {
          case 1: //创建
            color = "#13c2c2";
            break;
          case 3: //待审批  待处理
            color = "#2db7f5";
            break;
          case 4: //审批通过  已处理
            color = "#87d068";
            break;
          case 5: //审批未通过
            color = "#f50";
            break;
          case 6: //撤回  退回  被撤回  被退回  未生效
            color = "#ccc";
            break;
          case 7: //已关闭
            color = "#108ee9";
            break;
          default:
            color = "#000";
        }

        return color;
      },
      genStatusDesc(status){
        switch (status) {  //审批流状态
          case 1:   //AUDIT_CREATED
            return (this.$t("AuditTag.aabd54f1ee6111e9bd1088b1118b82a5"));
          case 2:  //AUDIT_DISTRIBUTION
            return (this.$t('AuditTag.aabd54f2ee6111e9bbb688b1118b82a5'));
          case 3:  //AUDIT_WAIT
            return (this.$t("AuditTag.aabd54f3ee6111e981b388b1118b82a5"));
          case 4:  //AUDIT_AGREE
            return (this.$t("AuditTag.aabd54f4ee6111e9ae6588b1118b82a5"));
          case 5:  //AUDIT_REFUSE
            return (this.$t("AuditTag.aabd54f5ee6111e9970188b1118b82a5"));
          case 6:  //AUDIT_REVOKED
            return (this.$t("AuditTag.aabd54f6ee6111e9848488b1118b82a5"));
          case 7:  //AUDIT_BACK
            return (this.$t("AuditTag.aabd54f7ee6111e99c1f88b1118b82a5"));
          case 8:  //AUDIT_IS_REVOKED
            return (this.$t("AuditTag.aabd54f8ee6111e9a0f588b1118b82a5"));
          case 9:  //AUDIT_IS_BACK
            return (this.$t("AuditTag.aabd54f9ee6111e9bde788b1118b82a5"));
          case 10:  //AUDIT_CLOSED
            return (this.$t("AuditTag.aabd7c18ee6111e99e1988b1118b82a5"));
          case 11:  //AUDIT_ERROR_CLOSED
            return (this.$t("AuditTag.aabd7c19ee6111e9894588b1118b82a5"));
          case 12:  //AUDIT_NOT_EFFECTIVE
            return (this.$t("AuditTag.aabd7c1aee6111e9834788b1118b82a5"));
          case 13:  //AUDIT_SUBMIT
            return (this.$t("AuditTag.aabd7c1bee6111e985dc88b1118b82a5"));
        }
      }

    },
    components: {
      "van-tag": Tag,
    }
  };
</script>
