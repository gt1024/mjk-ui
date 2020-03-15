<style>
.el-icon-plus{
    font-style: normal;
    font-weight: 800;
    font-variant: normal;
    text-transform: none;
    line-height: 1;
    vertical-align: baseline;
    display: inline-block;
    color:black!important;
    float: none !important;
}
.iconstyle{
    float: none !important;
    line-height: 1.8rem !important;
}
.iconstyle:before {
    font-size: 14px;
    display: inline-block;
    transform: rotate(180deg);
    float: none !important;
    line-height: 3.8rem;
}
.is-circle {
    border-radius: 50%;
    padding: 12px 0;
    height:40px!important;
    width:40px!important;
    font-size: 1rem !important;
}

.circle_span{
  overflow:hidden;
  width: 40px !important;
  /*width: 100% !important;*/
  float: none !important;
  margin: 0 auto;
  display: inline-block;
  /*margin-left: -5px;*/
}
.cornerx{
    color: white;
    height: 16px;
    width: 16px!important;
    left: 24px;
    top: -30px;
    /*line-height: 16px;*/
    float: none!important;
    position: relative;
    background: black;
    text-align: center;
    border-radius: 50%;
}

.spanx{
    float: none !important;
    width: 100%;
    position: relative;
    /*margin-left: -10px;*/
}

.vanPop{
  height: 30%;
}

.search_pop{
  height: 25%;
}

.picker{
  margin: 5% 0;
}

.cov{
	width:100%;
	height:100%;
	background-color:rgba(0, 0, 0, 0.5);
	z-index:1000;
	display:none;
	position:absolute;
	text-align:center;
	font-size: 16px;
	box-shadow:0px 0px 5px black;
}
.con {
  z-index: 1100;
  width: 600px;
  height: 200px;
  background-color: white;
  position: fixed;
  right: 30%;
  top: 30%;
  position: fixed;
  box-shadow: 0px 0px 15px black;
}
</style><template>
  <div>
    <!--绿色新添加的人-->
    <div style="display:inline" v-for="(item,index) in searchids" :key="index">
      <el-button class="is-circle" type="success" >
        <span class="circle_span">{{item.name}}</span>
        <div class="cornerx" v-on:click.stop="deleteitem(index)">
          <span class="spanx">x</span>
        </div>
      </el-button>
      <i class="iconfont icon-xitongfanhui iconstyle"></i>
    </div>

    <!--灰色之前添加的人-->
    <div style="display:inline" v-for="(item,index) in next_staff">
      <el-button class="is-circle" type="info" >
        <span class="circle_span">{{item.name| truncate(3)}}</span>
      </el-button>
      <i class="iconfont icon-xitongfanhui iconstyle" v-show="index!==next_staff.length-1||show_add===1"></i>
    </div>

    <!--圆心加号-->
    <el-button class="is-circle" icon="el-icon-plus" v-on:click="addShow" v-show="this.show_add===1"></el-button>

    <!--<van-popup v-model="show" position="bottom" :overlay="true" :close-on-click-overlay="true" class="vanPop">-->
     <mt-popup v-model="show" position="bottom" popup-transition="popup-fade" :closeOnClickModal="clickShow" class="search_pop">
       <mt-picker :slots="slots" :visible-item-count="3" v-on:change="pickerchange" valueKey="text"></mt-picker>
     </mt-popup>

     <mt-popup v-model="inputShow" pop-transition="pop-transition" style="height:0">
       <div class="mint-msgbox" style="">
         <div class="mint-msgbox-header">
           <div class="mint-msgbox-title">搜索</div>
         </div>
         <div class="mint-msgbox-content">
           <div class="mint-msgbox-message">输入用户姓名或座机</div>
           <div class="mint-msgbox-input" style="">
             <input placeholder="" type="text" v-model="inputVal">
             <div class="mint-msgbox-errormsg" style="visibility: hidden;"></div>
           </div></div> <div class="mint-msgbox-btns">
         <button class="mint-msgbox-btn mint-msgbox-cancel " style="" @click="inputShow=false">取消</button>
         <button class="mint-msgbox-btn mint-msgbox-confirm " @click="add">确定</button>
       </div>
       </div>
     </mt-popup>
        <!--</van-popup>-->
    <!--</div>-->

  </div>
</template><script>
import { Button,Icon } from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
import { Dialog } from "vant";
import Vue from "vue";
Vue.use(Button);
import { Toast, MessageBox } from "mint-ui";

export default {
	props: ["url","type","next","show_add"],
	data: function() {
		return {
			show: false,
			slots: [{ values: [] }],
			searchids:[],
			addnew:false,
			next_staff:[],
      clickShow:true,
      handler:function(e){e.preventDefault();},
      inputShow:false,
      inputVal:"",
		};
	},

	created(){
     console.log(this.show_add)
	},

	methods: {
		pickerchange: function(picker, values) {
		  // console.log(values)
			if (typeof values != "undefined" && values.length > 0 && values[0]) {
				let item={};
				item.name=values[0].text.substring(0,2);
				item.id=values[0].id;
				if(this.addnew){
					this.addnew=false;
					this.searchids.push(item);
				}else{
					this.searchids.splice(-1,1,item);
				}
				this.$emit("transfervalue", this.searchids.map(function(x){return x.id;}).join());

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

    addShow(){
		  this.inputShow=true
    },

    add() {
    	var that=this;
      let value=that.inputVal;
    	// MessageBox.prompt(
    	//   this.$t("multisearch.b5e2aca2974f11e9a5048c8590a7c397"),
      //   this.$t("multisearch.b5e2ad92974f11e9a5048c8590a7c397"),
      //   {confirmButtonText: this.$t('button.ok'), cancelButtonText: this.$t('button.cancel')},
      //   {showInput: true}
      //   ).then(({ value, action }) => {
          // console.log(action);
          // this.onBlur();

    		if(!value || value.trim()==""){
    			Toast(this.$t("multisearch.b5e2ae50974f11e9a5048c8590a7c397"));
    		}else{
          that.inputShow=false;
          that.inputVal='';

    		  this.slots=[{ values: [] }];

    			var params = {
    				q: value
    			};

    			if(that.type){
    				params.type=that.type;
    			}

    			that.$axios({
    				url: that.url,
    				method: "GET",
    				params: params
    				//            params: {q: value}
    			}).then(res => {
    					if (res.data.length !== 0) {
    						that.addnew=true;
    						that.show = true;
    						// this.closeTouch();
    						that.slots[0].values = res.data.map(function(x) {
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

    	// });
    },

		// add() {
		// 	var that=this;
		// 	MessageBox.prompt(
		// 	  this.$t("multisearch.b5e2aca2974f11e9a5048c8590a7c397"),
    //     this.$t("multisearch.b5e2ad92974f11e9a5048c8590a7c397"),
    //     {confirmButtonText: this.$t('button.ok'), cancelButtonText: this.$t('button.cancel')},
    //     {showInput: true}
    //     ).then(({ value, action }) => {
    //       // console.log(action);
    //       // this.onBlur();
    //
		// 		if(!value || value.trim()==""){
		// 			Toast(this.$t("multisearch.b5e2ae50974f11e9a5048c8590a7c397"));
		// 		}else{
		// 		  this.slots=[{ values: [] }];
    //
		// 			var params = {
		// 				q: value
		// 			};
    //
		// 			if(that.type){
		// 				params.type=that.type;
		// 			}
    //
		// 			that.$axios({
		// 				url: that.url,
		// 				method: "GET",
		// 				params: params
		// 				//            params: {q: value}
		// 			}).then(res => {
		// 					if (res.data.length !== 0) {
		// 						that.addnew=true;
		// 						that.show = true;
		// 						// this.closeTouch();
		// 						that.slots[0].values = res.data.map(function(x) {
		// 							x.text=x.name;
		// 							if(x.tel){
		// 								x.text+="-" + x.tel;
		// 							}
		// 							if(x.department){
		// 								x.text+="-" + x.department;
		// 							}
		// 							return x;
		// 						});
		// 					} else {
		// 						Toast(this.$t("multisearch.b5e2af18974f11e9a5048c8590a7c397"));
		// 					}
		// 				});
		// 		}
    //
		// 	});
		// },

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
	},

  components: {
    "van-dialog": Dialog,
  },
};
</script>
