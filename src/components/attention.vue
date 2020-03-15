<style scoped>
  .close_mt{
    margin: 0 auto;
    background-color: rgb(24,144,255);
  }

  table td{
    text-align: left;
  }

  .attention{
    padding-top: 0;
  }

  .carry_sub{
    height: auto;
    margin: 0;
    padding: 0;
  }

  .carry_sub button{
    margin: 0.5rem 0 !important;
  }

  .__word td{
    padding-top: 0;
  }
</style><template>
  <div>
     <mt-popup v-model="popupVisible3" position="right" class="mint-popup-3" >
      <div class="__word" style="overflow-y: auto;height: 95%">
        <table>
            <thead>
                <tr class="border-b">
                    <th class="center" colspan="6">
                        <p style="font-size:16px;">{{$t('attention.b5e11ef0974f11e9a5048c8590a7c397')}}</p>
                        <p style="font-size:16px;">Confirmation Letter of Reserve</p>
                    </th>
                </tr>
                <tr>
                    <td class=" padding-t8b12" colspan="6">
                        <p>{{$t('attention.b5e119e6974f11e9a5048c8590a7c397')}}</p>
                        <!--<p>Dear Reserve:</p>-->
                    </td>
                </tr>
            </thead>
            <tfoot>
                <tr class="border-b">
                    <td colspan="6" class="attention"></td>
                </tr>

                <tr>
                    <td colspan="6">
                      <button class="d_btn" style="margin-top: 8px" @click="popupVisible3 = false">{{$t('attention.b5e11158974f11e9a5048c8590a7c397')}}</button>
                    </td>
                </tr>
            </tfoot>
        </table>
       </div>
     </mt-popup>
  </div>
</template><script>
  /* eslint-disable indent,no-mixed-spaces-and-tabs,no-undef */

  export default {
  props: ["type"],
  data() {
  		return {
			//      popupVisible3:false,
  			popupVisible3:true,
  			info:{},
  		};},

  	created(){
  		this.$axios({
  			method: "GET",
  			url: "/v1/construction/project/attention/",
  		}
  		).then((res)=> {
	      switch (parseInt(this.type)){
	        case 1:$(".attention").html(res.data.work_permit);
  				break;
  			case 2:this.cleanXss(res.data.hanging);
  				break;
  			case 3:this.cleanXss(res.data.confined);
  				break;
  			case 4:this.cleanXss(res.data.high_work_permit);
  				break;
  			case 5:this.cleanXss(res.data.hot_work_permit);
  				break;
  			}
  		});
  },

  	methods: {
  		ask(){
  			this.$axios({
  				method: "GET",
  				url: "/v1/construction/project/attention/",
  			}
  			).then((res)=> {
  				// console.log(res);
  				localStorage.setItem("ask_info", res.data);

				//	      switch (parseInt(this.type)){
				//	        case 1:$('.attention').html(res.data.work_permit);
				//                break;
				//          case 2:this.cleanXss(res.data.hanging);
				//                break;
				//          case 3:this.cleanXss(res.data.confined);
				//                break;
				//          case 4:this.cleanXss(res.data.high_work_permit);
				//                break;
				//          case 5:this.cleanXss(res.data.hot_work_permit);
				//                break;
				//        }
  			});
  		},

  		cleanXss(value) {
  //        value = value.replace("<", "& lt;").replace(">", "& gt;");
  			value = value.replace("\\(", "& #40;").replace("\\)", "& #41;");
  			value = value.replace("'", "& #39;");
  			value = value.replace("eval\\((.*)\\)", "");
  			value = value.replace("[\\\"\\\'][\\s]*javascript:(.*)[\\\"\\\']", "\"\"");
  			value = value.replace("script", "");
  			$(".attention").html(value);
  //        return value;
  		}
	},

  };
</script>