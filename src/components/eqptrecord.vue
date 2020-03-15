<style>
  .step {
    line-height: 2rem;
    padding: 0 1rem;
    font-size: 1.2rem;
  }
  .step span{color: green;}
  .step .active {color: red;}
</style>
<template>
  <div>
    <van-cell :title='$t("eqptrecord.aac1ffccee6111e998ed88b1118b82a5")' is-link :value='$t("eqptrecord.aac1ffcdee6111e9b8a188b1118b82a5")' @click="showPopup"/>

    <van-popup
      position="right"
      v-model="show"
      :style="{ width: '100%',height: '100%' }"
    >
      <div class="g1_info">
        <i class="am-icon-send-o"></i>&nbsp;
        {{$t('eqptrecord.aac1ffceee6111e9b64088b1118b82a5')}}
      </div>
      <time-line
        v-for="(a, aIndex) of records"
        :key="aIndex"
      >
        <template slot="lists">
          <div>{{$t('facilitycheck.b5fc90b8974f11e9a5048c8590a7c397')}}:&nbsp;{{a.user_name}}</div>
          <!--<div>{{$t('eqptrecord.aac2135fee6111e9af3888b1118b82a5')}}:&nbsp;{{a.last_date | dateFormat}}</div>-->
          <!--<div>{{$t('eqptrecord.aac21360ee6111e9a32a88b1118b82a5')}}:&nbsp;{{a.next_date | dateFormat}}</div>-->
          <div>{{$t('detail.b6025e3a974f11e9a5048c8590a7c397')}}:&nbsp;{{a.status}}</div>
          <div v-if="a.status_id == 1 || a.status_id == 2">{{$t('facilitycheck.b5fc8c9e974f11e9a5048c8590a7c397')}}:&nbsp;
            <!--<div v-for="(item,index) of a.step_info">-->
            <!--<van-checkbox-group v-model="result">-->
            <!--<van-checkbox-->
            <!--disabled-->
            <!--:key="index"-->
            <!--:name="item"-->
            <!--&gt;-->
            <!--{{ item.name }}-->
            <!--</van-checkbox>-->
            <!--</van-checkbox-group>-->
            <!--</div>-->
            <!--<template v-if="Array.isArray(result)"></template>-->

            <template v-if="Array.isArray(a.step_info)">

              <div v-for="(item,index) of a.step_info">
                <div style="padding-left: 10px;">
                  <span>{{$t('eqptrecord.aac226f6ee6111e997fc88b1118b82a5')}}{{index+1}}：{{item.name}}</span>
                  <div v-for="(q,qIndex) of item.children" class="step">
                    <!--<div v-if="q.id == result[index][0].result_id">{{q.name}}</div>-->
                    <div v-if="q.id == a.result_info[index].result_id">{{$t('eqptrecord.aac21361ee6111e993f788b1118b82a5')}}<span :class="{'active': a.result_info[index].is_fault === 'True'}">{{q.name}}</span></div>
                  </div>
                </div>
              </div>
            </template>

          </div>
          <div>{{$t('facilitycheck.b5fc8f46974f11e9a5048c8590a7c397')}}:&nbsp;{{a.submit_time | timeFormat}}</div>
          <!--<div v-if="aIndex !== 0">{{$t('facilitycheck.b5fc9176974f11e9a5048c8590a7c397')}}:&nbsp;{{a.remarks ? a.remarks : "$t('facilitycheck.b5fb8ef2974f11e9a5048c8590a7c397')"}}</div>-->
        </template>
      </time-line>

      <template v-if="records.length === 0">
          <div class="nodata">
            <img src="../assets/nodata.png" height="212" width="235" alt="">
             <p style="margin: 0;font-family: Arial;font-size: 18px;color: #888888;">{{$t('app.table.nodata')}}</p>
          </div>
      </template>


      <div class="she_beizhu she_fujian">
        <button type="button" class="d_btn" style="margin:2rem 0" @click="closePopup">{{$t('eqptrecord.aac226f7ee6111e9bcd488b1118b82a5')}}</button>
      </div>

    </van-popup>
  </div>

</template>
<script>
  import moment from "moment";
  import {Collapse, CollapseItem, Cell, CellGroup, Panel, Checkbox, CheckboxGroup, Popup} from "vant";
  import TimeLine from "./TimeLine";

  export default {
    props: ["records", "result"],  // val - records
    data() {
      return {
        temp: "",
        activeTimes: [],
        list: ['a', 'b', 'c'],
        show: false
      };
    },
    mounted(){

    },
    created() {
      console.log(this.result)
      console.log(this.records)
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
      "van-popup": Popup
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
