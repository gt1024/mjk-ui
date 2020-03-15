<style scoped>
  /*check*/
  .exam {
    background-color: #ffffff;
    margin: 0 1rem;
    border-radius: 8px;
    padding: 0 10px;
  }

  .exam > ul {
    margin: 1rem 5px;
  }

  .exam .red {color: red;}
  .exam .green {color: green;}

  .notice {
    background-color: #ffffff;
    margin: 5px 1rem 0 1rem;
    border-radius: 8px;
    padding: 0.5rem 1rem;
  }

  .notice > p {
    margin: 0;
  }

  .notice > p:first-child {
    text-align: center;
  }

  .notice > p:last-child > span {
    display: block;
  }

  .mint-popup-3 {
    width: 100%;
    height: 100%;
    border: 1px solid transparent;
  }

  .mt_word {
    padding-top: 200px;
    height: 90%;
  }

  .mt_word > p {
    font-size: 20px;
    text-align: center;
  }

  .btn-area {
    width: 100%;
    box-sizing: border-box;
    padding: 0 1rem;
    font-size: 1.2rem;
    margin-bottom: 2px;
  }

  .area-child {
    height: 4rem;
    width: 100%;
    background: #59C4E6;
    color: #F2F2F2;
    border: 0;
    border-radius: 5px;
    /*margin: 13rem 0 2rem 0;*/
    vertical-align: middle;
    line-height: normal;
    overflow: visible;
    font: inherit;
    box-sizing: border-box;

  }

  .area-child2 {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .answer_p {
    margin: 0 0 1rem 1rem;
    white-space: nowrap;
  }
  .wrapper {
    overflow:auto;
     -webkit-overflow-scrolling:touch;
  }

</style>
<template>
  <div>
    <van-cell :title='$t("checkresult.aabfa1b8ee6111e98b2288b1118b82a5")' is-link :value='$t("checkresult.aabfa1b9ee6111e9acca88b1118b82a5")' @click="showPopup"/>

    <van-popup
      position="right"
      v-model="show"
      :style="{ width: '100%',height: '100%' }"
    >

      <div class="g1_info">
        <i class="am-icon-send-o"></i>&nbsp;{{$t('checkresult.aabfa1bcee6111e997b188b1118b82a5')}}</div>

      <div v-if="record && record.step_info">

        <div class="exam" v-if="record && record.step_info">
          <ul class="" v-for="(item,index) in record.step_info">
            <li class="exam_li">
              <p>{{index + 1}}.{{item.name}}</p>
              <p v-for="(choose,key) in item.children" class="answer_p">
                <input type="radio" v-if="choose.id == record.result_info[index].result_id" :name=item.id
                       :id="choose.id" checked
                       v-bind:value='choose.id+","+item.id' disabled>
                <input type="radio" v-else :name=item.id :id="choose.id"
                       v-bind:value='choose.id+","+item.id' disabled>
                <label :for=choose.id
                       v-bind:class="{red: choose.is_fault == 'True', green: choose.is_fault == 'False'}">{{choose.name}}</label>
                <span v-if="choose.has_remarks && record.result_info[index].remarks && choose.id == record.result_info[index].result_id" class="ipt-wrap" :style="{display: 'block',borderBottom: '1px solid #ebedf0', lineHeight: '1.4', margin: '0 15px 0 2rem'}">
                    {{record.result_info[index].remarks}}
                </span>
                <img-upload
                  url=''
                  v-if="choose.has_remarks && record.result_info[index].file_path && choose.id == record.result_info[index].result_id"
                  :defaultFileLists="
                    record.result_info[index].file_path ?
                      record.result_info[index].file_path.split(',').map(s => {
                        let fileName
                        const fileNameArr = /\/([^/]+)$/.exec(s)

                        if (fileNameArr) {
                          fileName = fileNameArr[1]
                        }

                        return {
                          id: s,
                          url: s,
                          fileName
                        }
                      }) : []
                  " noAdd />
                <!--<span v-if="choose.has_remarks" class="btn btn-xs"><van-icon name="van-icon van-icon-plus" /> 上传</span>-->
              </p>
            </li>
          </ul>
        </div>

      </div>

      <template v-else>
        <div class="nodata">
          <img src="../assets/nodata.png" height="212" width="235" alt="">
          <p style="margin: 0;font-family: Arial;font-size: 18px;color: #888888;">{{$t('app.table.nodata')}}</p>
        </div>
      </template>

      <div class="she_beizhu she_fujian">
        <button type="button" class="d_btn" style="margin:2rem 0" @click="closePopup">{{$t('checkresult.aabfc7acee6111e98f9188b1118b82a5')}}</button>
      </div>

    </van-popup>
  </div>

</template>
<script>
  import moment from "moment";
  import {Collapse, CollapseItem, Cell, CellGroup, Panel, Checkbox, CheckboxGroup, Popup} from "vant";
  import TimeLine from "./TimeLine";
  import ImgUpload from '@/components/upload'
  export default {
    props: ["record"],
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
      console.log(this.record)
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
      "img-upload": ImgUpload,
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
