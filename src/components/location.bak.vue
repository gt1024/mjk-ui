<style scoped>
  .slot_person {
    font-size: 16px;
    width: 80%;
    margin-top: 5px;
    display: inline-block;
    padding-left: 40%;
  }

  .slot_confirm {
    font-size: 16px;
    color: #26a2ff;
    width: 19%;
    text-align: center;
    float: right;
    margin-top: 5px;
  }

  .picker {
    line-height: 1.8rem !important;
  }
</style>

<template>
  <div>
    <input type="text" placeholder="请选择区域" @click="popupLocation=true" v-model="area" readonly :disabled="dis_location"/>

    <div>
      <mt-popup v-model="popupLocation" position="bottom" popup-transition="popup-fade" closeOnClickModal="true" model="true" class="search_pop">
        <mt-picker :slots="dataList" ref="picker2" :visible-item-count="3" :valueKey=this.type :showToolbar="true" @change="onAddressChange">
          <span class="slot_person">请选择地址</span>
          <span class="slot_confirm" @click="addressConfirm">确认</span>
        </mt-picker>
      </mt-popup>
    </div>
  </div>
</template>

<script>
export default {
	props: ["address","type","slot_num","holder","location","factory"],
	data() {
		return {
			result: [{},{},{}],
			area:"",
			popupLocation:false,
			isFirstSelect:true,
			myAdress:null,
			dis_location:true,
			findex:0,
			aindex:0,
		};
	},

	created(){
		this.area=this.holder;

		setTimeout(() =>{
		  console.log(this.addressSlots);
			if(this.address.length>0) {
				this.addressSlots[0].defaultIndex = 0;
				this.addressSlots[2].defaultIndex = 0;
				this.dis_location=false;
			}
		}, 3000);
	},

	computed: {
		dataList() {
			if (this.address.length > 0) {
				this.addressSlots = [
					{
						flex: 1,
						values: this.address,
						className: "slot1",
						textAlign: "center",
						defaultIndex: 999,
					},
					{
						divider: true,
						content: "-",
						className: "slot2"
					},
					{
						flex: 1,
						values: this.address[0].children,
						className: "slot3",
						textAlign: "center",
						defaultIndex: 999
					}
				];
				return this.addressSlots;
			}
		}
	},

	mounted () {

	},

	watch:{
		factory: function (newValue) {
		  console.log(newValue);
			this.findex=this.addressSlots[0].values.map(a => a.id).indexOf(newValue);
			this.addressSlots[0].defaultIndex = this.findex;
			this.result[0]=this.address[this.findex];
		},

		location: function (newValue) {
			var aindex=this.address[this.findex].children.map(a => a.id).indexOf(newValue);
			this.addressSlots[2].defaultIndex = aindex;
			this.result[1]=this.address[this.findex].children[aindex];
			if(this.result[0]&&this.result[1]){
			  if(this.type === "number"){
			    this.area=this.result[0].number+"-"+this.result[1].number;
        }else{
			    this.area=this.result[0].name+"-"+this.result[1].name;
        }
				// this.area=this.result[0].name+"-"+this.result[1].name;
			   this.$emit("transfervalue",[this.result[0].id,this.result[1].id]);
			}
		}
	},

	methods: {
		onAddressChange(picker, values) {
		  console.log(values);
		  this.result[1]=new Object();
			var pickervalues = this.address;
			// console.log(values);
			if (values[0]) {
				let is_left = pickervalues.find(x => {
					return x.id == values[0].id;
				});

				if(is_left){
					picker.setSlotValues(1, values[0].children);
					this.result[0]=values[0];
				}else {
          this.result[1]=values[0];
				}
			}

			if(values[1]){
				this.result[1]=values[1];
			}else{
			  // this.result[1]=new Object();
      }
		},

		addressConfirm() {
		  console.log(this.result);
		  if(this.type === "number"){
		    this.area=this.result[0].number+"-"+this.result[1].number;
			}else{
		    this.area=this.result[0].name+"-"+this.result[1].name;
      }
			this.$emit("transfervalue",[this.result[0].id,this.result[1].id]);
			this.popupLocation=false;
		}
	}
};
</script>
