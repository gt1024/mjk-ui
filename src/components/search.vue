<style scoped>
  .s_btn{
    padding: 1px;
    width: 4em;
  }
  .search_span{
    width: 100% !important;
    margin-top: -6px;
  }

  .picker{
    margin: 5% 0 !important;
  }
</style><template>
<div>
    <div>
      <font>{{text}}</font>
      <input type="text" v-bind:placeholder="placeholder" v-model="searchtext" style="width:45%"/>
       <!--<button type="button" class="s_btn" v-on:click="search">{{$t('search.b5e26026974f11e9a5048c8590a7c397')}}</button>-->
      <button type="button" class="s_btn" v-on:click="search">
        <span class="search_span">{{$t('search.b5e26026974f11e9a5048c8590a7c397')}}</span>
      </button>
    </div>

    <div>
      <mt-popup v-model="show" position="bottom" popup-transition="popup-fade" closeOnClickModal="true" model="true" class="search_pop">
        <mt-picker :slots="slots" :show-toolbar="false" :visible-item-count="3" v-on:change="pickerchange" valueKey="text"></mt-picker>
      </mt-popup>
    </div>
</div>
</template><script>
import { Toast, MessageBox,Actionsheet } from "mint-ui";
export default {
	props: ["placeholder", "text","url","type","name"],
	data: function() {
		return {
			show: false,
			slots: [{ values: [] }],
			value_id: "",
			value_text: "",
			searchtext:""
			//      searchtext: ""
		};
	},

	created(){
		//    console.log(this.name)
	},

	watch: {
		name: function (newValue, oldValue) {
			this.searchtext = newValue;
		},
		searchtext:function (newVal) {
			this.$emit("transferTxt", newVal);
		}
	},

	methods: {
		pickerchange: function(picker, values) {
			if (typeof values != "undefined" && values.length > 0 && values[0]) {
				if(this.url==="/v1/construction/contractor/info/") {
					this.value_id = values[0].contractor_id;
					this.value_text = values[0].text;
					this.searchtext = values[0].text;
					this.$emit("transfervalue", this.value_id);
				}else {
					this.value_id = values[0].id;
					this.value_text = values[0].text;
					this.searchtext = values[0].text;
					this.$emit("transfervalue", this.value_id);
				}
			}
		},

		search: function() {
			if (!this.searchtext) {
				Toast(this.$t("search.b5e25cca974f11e9a5048c8590a7c397"));
				return;
			}
			var params = {
				q: this.searchtext
			};

			if(this.type){
				params.type=this.type;
			}

			this.$axios({
				url: this.url,
				method: "GET",
				params: params
			}).then(res => {
					if (res.data.length !== 0) {
						this.show = true;
						if(this.url==="/v1/construction/contractor/info/"){
							this.slots[0].values = res.data.map(function(x) {
								x.text=x.contractor_company +"-"+x.contractor_name;
								return x;
							});
						}else{
							this.slots[0].values = res.data.map(function(x) {
								x.text=x.name;
								if(x.tel){
									x.text+="-" + x.tel;
								}
								if(x.department){
									x.text+="-" + x.department;
								}
								return x;
							});
						}
					} else {
						this.clear();
						Toast(this.$t("search.b5e25f36974f11e9a5048c8590a7c397"));
					}
				})
				.catch(err => {});
		},
		clear(){
			this.value_id="";
			this.value_text="";
			this.$emit("transfervalue", this.value_id);
		}
	}
};
</script>
