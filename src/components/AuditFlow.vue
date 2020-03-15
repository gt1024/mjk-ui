<style>
  .van-step:last-child .van-step__line {
      width: 0;
  }

  .van-step .van-step__circle {width: 9px; height: 9px;}

  .van-step--vertical .van-step__line {top: 16px;left: -15px;background-color: #cccccc;}

  /*.van-step--vertical .van-icon-checked {left: -18px;}*/

  .van-step--finish .van-step__circle, .van-step--vertical.van-step--finish .van-step__line {
      background-color: #38f;
  }
</style>
<template>
  <div>
    <van-cell :title='$t("AuditFlow.d5a03deeefb611e98d4a88b1118b82a5")' is-link :value='$t("AuditFlow.d5ae8c74efb611e98c4988b1118b82a5")' @click="showPopup"/>

    <van-popup
      position="right"
      v-model="show"
      :style="{ width: '100%',height: '100%' }"
    >
      <div class="g1_info">
        <i class="am-icon-send-o"></i>&nbsp;{{$t('AuditFlow.d5ae8c77efb611e996cb88b1118b82a5')}}</div>

      <van-steps
        direction="vertical"
        :active="audit.length"
        active-color="#38f"
      >
        <van-step v-for="(item, index) of audit" :key="index">
          <h3>{{item.operation_name}}&nbsp;&nbsp;{{item.status}}</h3>
          <p>{{item.operation_time}}</p>
        </van-step>
        <van-step v-for="(a, aIndex) of next" :key="aIndex">
          <h3>{{a.name}}&nbsp;&nbsp;{{$t('AuditFlow.d5ae8c78efb611e9a72488b1118b82a5')}}</h3>
        </van-step>
      </van-steps>

      <div class="she_beizhu she_fujian">
        <button type="button" class="d_btn" style="margin:2rem 0" @click="closePopup">{{$t('AuditFlow.d5ae8c79efb611e9b90488b1118b82a5')}}</button>
      </div>

    </van-popup>
  </div>

</template>
<script>
  import moment from "moment";
  import {Collapse, CollapseItem, Cell, CellGroup, Panel, Checkbox, CheckboxGroup, Popup, Step, Steps} from "vant";
  import TimeLine from "./TimeLine";

  export default {
    props: ["audit", "next"],
    data() {
      return {
        temp: "",
        show: false
      };
    },
    created() {

    },
    methods: {
      showPopup() {
        this.show = true;
      },
      closePopup() {
        this.show = false;
      },
    },
    components: {
      "van-collapse": Collapse,
      "van-collapse-item": CollapseItem,
      "van-cell": Cell,
      "van-cell-group": CellGroup,
      "time-line": TimeLine,
      "van-panel": Panel,
      "van-checkbox-group": CheckboxGroup,
      "van-checkbox": Checkbox,
      "van-popup": Popup,
      "van-steps": Steps,
      "van-step": Step,
    },
    computed: {
//      myValue() {
//          return this.$store.state.data
//      }
    },
//    watch: {
//      val: function (newVal, oldVal) {
//        //其他业务代码
//        return newVal ? this.records = newVal : this.records = oldVal
//      }
//    },
    filters: {
      dateFormat: function (c) {
        return c ? moment(c).format("YYYY-MM-DD") : '-'
      },
      timeFormat: function (c) {
        return moment(c).format("YYYY-MM-DD HH:mm")
      }
    }
  };
</script>
