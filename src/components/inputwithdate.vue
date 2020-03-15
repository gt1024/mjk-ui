<style scoped>
  .mint-popup{
    height: 40%;
  }

  input{
    width:55% !important;
    /*width: auto !important;*/
  }

  .am-icon-calendar,.am-icon-clock-o{
    margin-right: 6px;
  }

  /*.date_pop{*/
    /*z-index:2001 !important;*/
  /*}*/
</style><template>
  <div>
    <div v-if="this.type=='date'">
      <font>{{text}}</font>
      <input type="text" readonly="readonly" :placeholder='$t("inputwithdate.b5e16694974f11e9a5048c8590a7c397")' v-model="date_info" v-on:click="openpicker('picker1')" :id="this.tran_id1" :disabled="this.dis_date"/>
      <mt-datetime-picker class="date_pop" ref="picker1" type="date" :startDate="now_date" :endDate='future_date' @confirm="handleConfirm1" :visible-item-count="5" v-model="value_d" :confirmText="$t('app.component.button.ok')" :cancelText="$t('app.component.button.cancel')" ></mt-datetime-picker>
      <i class="am-icon-calendar"></i>
    </div>

    <div v-else-if="this.type=='time'">
      <font>{{text}}</font>
      <input type="text" readonly="readonly" :placeholder='$t("inputwithdate.b5e1951a974f11e9a5048c8590a7c397")' v-on:click="openpicker('picker2')" :id="this.tran_id2" :disabled="this.dis_time"/>
      <mt-datetime-picker class="date_pop" ref="picker2" type="time" :startHour="now_time"  @confirm="handleConfirm2" :visible-item-count="5" v-model="value_t" :confirmText="$t('app.component.button.ok')" :cancelText="$t('app.component.button.cancel')" ></mt-datetime-picker>
      <i class="am-icon-clock-o"></i>
    </div>

    <div v-else>
      <font>{{text}}</font>
      <input type="text" readonly="readonly" :placeholder='$t("inputwithdate.b5e1b0d6974f11e9a5048c8590a7c397")' v-on:click="openpicker('picker3')" :id="this.tran_id3" v-model="dateTime_info" :disabled="this.dis_dt"/>
      <mt-datetime-picker class="date_pop" ref="picker3" @confirm="handleConfirm3" :visible-item-count="5" v-model="value_dt" :confirmText="$t('app.component.button.ok')" :cancelText="$t('app.component.button.cancel')" ></mt-datetime-picker>
      <i class="am-icon-calendar"></i>
    </div>
  </div>
</template><script>
import "../styles/weui.min.css";
import weui from "weui.js";
import moment from "moment";

export default {
	props: ["text","type","start_date","start_time","end_date","close","clear_time","end","original_date","original_time","tran_id1","tran_id2","tran_id3","original_dateTime","disabled_date","disabled_time","disabled_dt"],
	data() {
		return {
			value_d:"",
			value_t:"",
			value_dt: "",
			date_info:"",
			dateTime_info:"",
			now_date:new Date("2008-01-01"),
			now_time:0,
			future_date:new Date("2028-12-31"),
			dis_date:false,
			dis_time:false,
			dis_dt:false,
		};
	},

	created(){
	  console.log(this.original_time)

		if(this.disabled_date){
			this.dis_date=true;
		}

		if(this.disabled_time){
			this.dis_time=true;
		}

		if(this.disabled_dt){
			this.dis_dt=true;
		}
		//日期默认值
		setTimeout(() =>{
			if(this.original_date){
				if(this.original_date[0]){
					if(this.original_date[1]===0){
						if(typeof (this.original_date[0])==="object") {
							this.value_d = moment(this.original_date[0]).format("YYYY-MM-DD");
							//          $('#' + this.tran_id1).val(this.value_d);
							this.date_info=this.value_d;
							this.$emit("transfervalue", this.value_d);
						}else{
							this.value_d=this.original_date[0];
							//          $('#' + this.tran_id1).val(this.value_d);
							this.date_info=this.value_d;
							this.$emit("transfervalue", this.value_d);
						}
					}else{
						if(typeof (this.original_date[0])==="string"){
							var now=new Date(Date.parse(this.original_date[0].replace(/-/g, "/")));
						}else{
							var now=this.original_date[0];
						}
						var date=now.getDate();
						date=date+this.original_date[1];
						now.setDate(date);
						this.value_d = moment(now).format("YYYY-MM-DD");
						this.date_info=this.value_d;
						this.$emit("transfervalue", this.value_d);
					}
				}
			}
		},2000);


		if(this.original_dateTime){
			if(this.original_dateTime[0]){
				if(this.original_dateTime[1]===0){
					if(typeof (this.original_dateTime[0])==="object") {
						this.value_dt = moment(this.original_dateTime[0]).format("YYYY-MM-DD h:mm:ss");
						$("#"+this.tran_id3).val(this.value_dt);
						this.$emit("transfervalue", this.value_dt);
					}else{
						this.value_dt=this.original_dateTime[0];
						this.dateTime_info=this.value_dt;
						this.$emit("transfervalue", this.value_dt);
					}
				}
			}
		}

		setTimeout(() =>{
      if(this.original_time){
        if(this.original_time[1]===0){
          console.log(1);
          this.value_t = this.original_time[0];
          $("#"+this.tran_id2).val(this.value_t);
          // this.$emit("transfervalue", this.value_t);
        }
      }
		},1000);
	},


	methods: {
		openpicker(num) {
			this.$refs[num].open();
		},

		//    date
		handleConfirm1(value) {
			this.value_d = moment(value).format("YYYY-MM-DD");
			//      $('#'+this.tran_id1).val(this.value_d);
			this.date_info=this.value_d;
			this.$emit("transfervalue", this.value_d);
		},

		//    time
		handleConfirm2(value) {
			this.value_t=value;
			$("#"+this.tran_id2).val(this.value_t);
			this.$emit("transfervalue", this.value_t);
		},

		//    datetime
		handleConfirm3(value) {
			this.value_dt = moment(value).format("YYYY-MM-DD h:mm:ss");
			//      $('#'+this.tran_id3).val(this.value_dt);
			this.dateTime_info=this.value_dt;
			this.$emit("transfervalue", this.value_dt);
		},
	},

	watch: {
		//    日期初始值
		start_date: function (newValue, oldValue) {
			this.dis_date=false;
			if (newValue) {
				if(typeof (newValue)==="object") {
					this.now_date = newValue;
				}else{
					var date = new Date(Date.parse(newValue.replace(/-/g, "/")));
					this.now_date = date;
				}
			}
		},

		//    时间初始值
		start_time:function (newValue, oldValue) {
			if(newValue[0]){
				this.dis_time=false;
				this.now_time=parseInt(newValue[0]);
			}
		},

		//日期结束值
		end:function (newValue) {
			if(newValue[0]){
				if(typeof (newValue[0])==="string"){
					var getNow=new Date(Date.parse(newValue[0].replace(/-/g, "/")));
					var date_old=getNow.getDate();
					date_old+=newValue[1];
					getNow.setDate(date_old);
					this.future_date=getNow;
				}else{
					var getNow=new Date();
					var date_old=getNow.getDate();
					date_old+=newValue[1];
					getNow.setDate(date_old);
					this.future_date=getNow;
				}
			}
		},

		clear_time:function (newValue) {
			this.value_t="";
			this.value_dt="";
			$("#"+this.tran_id2).val("");
			$("#"+this.tran_id3).val("");
		},

	}
};
</script>
