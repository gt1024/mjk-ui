<style>
  .search .cell-value {
    width: 72%;float: left;overflow: hidden;text-overflow : ellipsis;white-space: nowrap;text-align: left;
  }
.van-icon.van-icon-search{
  float: right;
  margin-right:6px;
}
.s_btn{
  line-height: 1rem;
  padding: 1px;
  width: 4em;
}
</style>
<template>
  <div class="search">

    <!--<van-icon-->
      <!--v-on:click.stop="add"-->
      <!--name="search"-->
      <!--style="line-height: inherit;"-->
    <!--/>-->

    <button type="button" class="s_btn" v-on:click.stop="add">
      <span class="search_span">{{$t('search.b5e26026974f11e9a5048c8590a7c397')}}</span>
    </button>

    <div class="cell-value">
        <span class="">{{searchids.name}}</span>
    </div>

     <mt-popup v-model="show" position="bottom" popup-transition="popup-fade" :closeOnClickModal="clickShow" class="search_pop">
       <mt-picker :slots="slots" :visible-item-count="3" v-on:change="pickerchange" valueKey="text"></mt-picker>
     </mt-popup>

  </div>
</template>
<script>
import { Button } from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
import Vue from "vue";
Vue.use(Button);
import { MessageBox } from "mint-ui";
import {Icon, Popup, Toast} from "vant";

export default {
	props: ["url","type","next", "searchids"],
	data: function() {
		return {
			show: false,
			slots: [{ values: [] }],
			//searchids: {},

			next_staff:[],
      clickShow:true,
      handler:function(e){e.preventDefault();},
		};
	},

	created(){

	},
  components: {

      "van-icon": Icon,
    },
	methods: {
		pickerchange: function(picker, values) {
		   console.log(values)
			if (typeof values != "undefined" && values.length > 0 && values[0]) {
				let item={};
				item.name=values[0].text;
				item.id=values[0].id;
        console.log(item)
        //this.searchids.push(item);
        this.searchids = item
				//this.$emit("transfervalue", this.searchids.map(function(x){return x.id;}).join());
				this.$emit("transfervalue", this.searchids.id);

			}
		},

    onFocus(val,$event){
		  var body=document.getElementsByTagName("body");
		  body.scrollTop=body.scrollHeight;
    },

    onBlur(val,$event){
		  $(".content").scrollTo(0,0);
		  // window.scrollTo(0,0)
    },

    closeTouch:function(){
        document.getElementsByTagName("body")[0].addEventListener('touchmove',
            this.handler,{passive:false});//阻止默认事件
        console.log("closeTouch haved happened.");
    },
    openTouch:function(){
        document.getElementsByTagName("body")[0].removeEventListener('touchmove',
            this.handler,{passive:false});//打开默认事件
        console.log("openTouch haved happened.");
    },

		add() {
			MessageBox.prompt(
			  this.$t("multisearch.b5e2aca2974f11e9a5048c8590a7c397"),
        this.$t("multisearch.b5e2ad92974f11e9a5048c8590a7c397"),
        {confirmButtonText: this.$t('button.ok'), cancelButtonText: this.$t('button.cancel')},
        {showInput: true}
        ).then(({ value, action }) => {
          // console.log(action);
          // this.onBlur();

				if(!value || value.trim()==""){
					Toast(this.$t("multisearch.b5e2ae50974f11e9a5048c8590a7c397"));
				}else{
				  this.slots=[{ values: [] }];

					var params = {
						q: value
					};

					if(this.type){
						params.type=this.type;
					}

					this.$axios({
						url: this.url,
						method: "GET",
						params: params
						//            params: {q: value}
					}).then(res => {
							if (res.data.length !== 0) {

								this.show = true;
								// this.closeTouch();
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
							} else {
								Toast(this.$t("multisearch.b5e2af18974f11e9a5048c8590a7c397"));
							}
						});
				}

			});
		},

		clear() {
			this.searchids=[];
			this.$emit("transfervalue", this.searchids.map(function(x){return x.id;}).join());
		},

		deleteitem(index){
			this.searchids.splice(index,1);
			this.$emit("transfervalue", this.searchids.map(function(x){return x.id;}).join());
		},

	},

	watch:{
		next:function (newValue) {
			for(var i=0;i<newValue.length;i++){
				if(newValue[i].type===newValue[0].type){
					this.next_staff.push(newValue[i]);
				}
			}
		},
	}
};
</script>
