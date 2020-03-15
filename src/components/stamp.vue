<style>

</style>
<template>
  <div>
    <div class="stamp">
      <i v-if="this.state==1" class="iconfont icon-yixiajiatuzhang" style="color:cyan"></i>
      <i v-else-if="this.state==2" class="iconfont icon-yixiajiatuzhang"></i>
      <i v-else-if="this.state==3" class="iconfont icon-yixiajiatuzhang" style="color:#2db7f5"></i>
      <i v-else-if="this.state==4" class="iconfont icon-yixiajiatuzhang" style="color:#87d068"></i>
      <i v-else-if="this.state==5" class="iconfont icon-yixiajiatuzhang" style="color:#f50"></i>
      <i v-else-if="this.state==6" class="iconfont icon-yixiajiatuzhang" style="color:#CCCCCC"></i>
      <i v-else-if="this.state==7" class="iconfont icon-yixiajiatuzhang" style="color:#CCCCCC"></i>
      <i v-else-if="this.state==8" class="iconfont icon-yixiajiatuzhang" style="color:#CCCCCC"></i>
      <i v-else-if="this.state==9" class="iconfont icon-yixiajiatuzhang" style="color:#CCCCCC"></i>
      <i v-else-if="this.state==10" class="iconfont icon-yixiajiatuzhang" style="color:#108ee9"></i>
      <i v-else-if="this.state==11" class="iconfont icon-yixiajiatuzhang" style="color:#f50"></i>
      <i v-else-if="this.state==12" class="iconfont icon-yixiajiatuzhang" style="color:#CCCCCC"></i>
      <i v-else="this.state==13" class="iconfont icon-yixiajiatuzhang"></i>
      <span>{{state | statusChar}}</span>
    </div>
  </div>
</template>
<script>
  import i18n from '@/utils/lang'

  export default {
    props: ["state"],
    data: function () {
      return {};
    },

    filters: {
      //后台传来的number类型的状态改为字符串类型的
      "statusChar": function (num) {
//        var status={
//   				1: "创建", 2: "待分配", 3: "待审批", 4: "审批通过",
//   				5: "审批未通过", 6: "撤回", 7: "退回", 8: "被撤回", 9: "被退回",
//   				10: "已关闭", 11: "异常关闭", 12: "未生效", 13: "提交"
//   			};
        var status = {
          1: i18n.t("stamp.b5e39da6974f11e9a5048c8590a7c397"),
          2: i18n.t("stamp.b5e39e8c974f11e9a5048c8590a7c397"),
          3: i18n.t("stamp.b5e39f36974f11e9a5048c8590a7c397"),
          4: i18n.t("stamp.b5e39fcc974f11e9a5048c8590a7c397"),
          5: i18n.t("stamp.b5e3a058974f11e9a5048c8590a7c397"),
          6: i18n.t("stamp.b5e3a0e4974f11e9a5048c8590a7c397"),
          7: i18n.t("stamp.b5e3a170974f11e9a5048c8590a7c397"),
          8: i18n.t("stamp.b5e3a1fc974f11e9a5048c8590a7c397"),
          9: i18n.t("stamp.b5e3a288974f11e9a5048c8590a7c397"),
          10: i18n.t("stamp.b5e3a30a974f11e9a5048c8590a7c397"),
          11: i18n.t("stamp.b5e3a396974f11e9a5048c8590a7c397"),
          12: i18n.t("stamp.b5e3a422974f11e9a5048c8590a7c397"),
          13: i18n.t("stamp.b5e3a4ae974f11e9a5048c8590a7c397")
        };

        for (var key in status) {
          if (Number(key) === num) {
            return status[key];
          }
        }
      }
    },

  };
</script>
