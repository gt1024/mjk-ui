<style scoped>
  .iconfont{
    font-size: 20px !important;
    display: block;
    line-height: 1;
    color:#808080;
    margin-top: 5px;
  }

  .clear{clear:both;height:0;line-height:0;font-size:0;visibility:hidden;overflow:hidden}

  .clearfix:after{visibility: hidden;display: block;font-size: 0;content: " ";clear: both;height: 0;zoom:1;}
  .l{float:left;}.r{float:right;}

  .btn2{
    height: 44px;
  }

  .btn3{
    position:fixed;
    /*z-index: -1;*/
    z-index:3;
    bottom:0px;
    border-top:1px solid #ebebeb;
    background:#e6e6e6;
    width:100%;
    text-align:center;
    box-sizing:border-box;
    -webkit-box-sizing:border-box;
  }

  .menu{position:relative;
      float:left;
      width:25%;
      /*height:50px;*/
      line-height:1.4;
      background:#fff;
      /*border-right:1px solid #ebebeb;*/
      box-sizing:border-box;
      -webkit-box-sizing:border-box;}

  .menu:last-child{border-right:none;}

  .new-sub{position:absolute;
        bottom:60px;
        z-index:10;
        padding: 0px 10px;
        background: #fff;
        box-sizing:border-box;
        -webkit-box-sizing:border-box;
        border: 1px solid #EEEEEE;
        border-radius: 5px;
        /*display:none;*/
  }
  .new-sub li{width: 100%;
          background:#fff;
          float:none;
          box-sizing:border-box;
          -webkit-box-sizing:border-box;
          border-top:1px solid #f2f2f2;}
  .new-sub li a{display:block;
            /*height:50px;*/
            line-height:40px;
            text-align:left;
            background:#fff;
            color:#333;
            border:none;
            text-align:center;
            font-size:1.2rem;}

    .sanjiao{
      position:absolute;
      bottom:5px;
      right:5px;
      width:0;
      height:0;
      border:5px solid transparent;
      border-right:5px solid #808080;
      border-bottom:5px solid #808080;
      opacity:.5;}

    .bt-name{
      color:#808080;
    }

    .bt-name a{display:block;font-size:16px;color:#000;}

    .new-sub .tiggle{
      width:0px;
          height:0px;
          position:absolute;
          left:50%;
          margin-left:-10px;
          bottom:-9px;
          border-top:10px solid #EEEEEE;
          border-left:10px solid transparent;
          border-right:10px solid transparent;
          z-index:10;
      }

      .new-sub .innertiggle{
        width:0px;
          height:0px;
          position:absolute;
          left:50%;
          margin-left:-9px;
          bottom:-8px;
          border-top:9px solid white;
          border-left:9px solid transparent;
          border-right:9px solid transparent;
          z-index:11;
      }

      .bt-name span{
        font-size: 0.85rem;
      }

      .tab_new{
        height: 44px;
      }

      .bt-name3{
        position: relative;
      }

      .tip_red {
        position: absolute;
        min-width: 16px;
        height: 16px;
        background: red;
        box-sizing: border-box;
        color: white;
        font-size: 8px;
        text-align: center;
        line-height: 16px;
        /*padding: 0 5px;*/
        border-radius: 8px;
        display: inline-block;
        right: 26px;
        top: -4px;
        }
</style>
<template>
  <!--<div>-->
      <!--<div class="btn2"></div>-->
      <div class="btn3 clearfix">
        <!--tab1-->
        <div class="menu">
            <div class="bt-name" @click="menu_toggle1()">
                <i class="iconfont icon-zhuye1"></i>
                <span>{{$t('tabbar.b5e4dce8974f11e9a5048c8590a7c397')}}</span>
            </div>
        </div>

         <!--tab2-->
        <div class="menu" @click.stop="menu_toggle2()" >
            <div class="bt-name bt-name2" id="tab2">
              <i class="iconfont icon-shenqing1"></i>
              <span>{{$t('tabbar.b5e4df04974f11e9a5048c8590a7c397')}}</span>
            </div>
            <!--<div class="sanjiao sanjiao2"></div>-->

            <div class="new-sub" v-show="this.tab2==true" id="son2">
                <ul>
                    <li v-show="this.result1.indexOf('/construction/project')>-1&&this.staff_type">
                      <router-link :to="{name:'construct_attention'}">{{$t('tabbar.b5e4c9c4974f11e9a5048c8590a7c397')}}</router-link>
                    </li>
                    <li v-show="this.result2.indexOf('/hotline/order')>-1">
                      <router-link :to="{path:'/workorder/newwork'}">{{$t('tabbar.b5e4cc58974f11e9a5048c8590a7c397')}}</router-link>
                    </li>
                    <li v-show="this.result3.indexOf('/carryout/goodsrecord')>-1">
                      <router-link :to="{path:'/carryout/newcarryout'}">{{$t('tabbar.b5e4cec4974f11e9a5048c8590a7c397')}}</router-link>
                    </li>
                    <li v-show="this.result4.indexOf('/event/appointment')>-1&&this.visitor===1">
                      <router-link :to="{path:'/visitor/addvisitors'}">{{$t('tabbar.b5e4d14e974f11e9a5048c8590a7c397')}}</router-link>
                    </li>
                </ul>
                <div v-show="this.result1.indexOf('/construction/project')>-1||this.result2.indexOf('/hotline/order')>-1||this.result3.indexOf('/carryout/goodsrecord')>-1||this.result4.indexOf('/event/appointment')>-1" class="tiggle"></div>
                <div v-show="this.result1.indexOf('/construction/project')>-1||this.result2.indexOf('/hotline/order')>-1||this.result3.indexOf('/carryout/goodsrecord')>-1||this.result4.indexOf('/event/appointment')>-1" class="innertiggle"></div>
            </div>
        </div>

         <!--tab3-->
        <div class="menu" @click.stop="menu_toggle3()">
         <div class="bt-name bt-name3" id="tab3">
           <i class="iconfont icon-ling"></i>
           <span class="tip_red" v-show="this.num>0">{{num>99?"99+":num}}</span>
           <span>{{$t('tabbar.b5e4e12a974f11e9a5048c8590a7c397')}}</span>
         </div>
        <!--<div class="sanjiao sanjiao3"></div>-->
        <div  class="new-sub" v-show="this.tab3==true" id="son3">
            <ul>
                <li>
                  <router-link :to="{name:'system_news'}">{{$t('tabbar.b5e4e12a974f11e9a5048c8590a7c397')}}<span v-show="this.num1>0">({{num1>99?"99+":num1}})</span></router-link>
                </li>
                <li>
                  <router-link :to="{name:'system_deal'}">{{$t('tabbar.b5e4e3fa974f11e9a5048c8590a7c397')}}<span v-show="this.num2>0">({{num2>99?"99+":num2}})</span></router-link>
                </li>
            </ul>
            <div class="tiggle"></div>
            <div class="innertiggle"></div>
        </div>
        </div>

        <!--tab4-->
        <div class="menu" @click.stop="menu_toggle4()">
         <div class="bt-name bt-name4" id="tab4">
           <i class="iconfont icon-renwu"></i>
           <span>{{$t('tabbar.b5e4e62a974f11e9a5048c8590a7c397')}}</span>
         </div>
        <!--<div class="sanjiao sanjiao4"></div>-->
        <div  class="new-sub" v-show="this.tab4==true" id="son4">
            <ul>
                <li>
                  <router-link :to="{path:'/system/me'}">{{$t('tabbar.b5e4d3a6974f11e9a5048c8590a7c397')}}</router-link>
                </li>
                <li>
                  <router-link :to="{path: '/system/changepassword'}">{{$t('tabbar.b5e4d5fe974f11e9a5048c8590a7c397')}}</router-link>
                </li>
                <li>
                  <a @click="changeLanguage" style="display: flex;justify-content: center;align-items: center;">{{this.lang == "en" ? "CN" : "EN"}}&nbsp;&nbsp;<img v-if="this.lang =='en'" src="../images/locales/china.png" alt=""><img v-if="this.lang =='zh'" src="../images/locales/uk.png" alt=""></a>
                </li>
                <!--<li>-->
                  <!--<router-link :to="{path:'/system/applypermission'}">{{$t('tabbar.b5e4d84c974f11e9a5048c8590a7c397')}}</router-link>-->
                <!--</li>-->
                <li>
                  <a @click="logout">{{$t('tabbar.b5e4da90974f11e9a5048c8590a7c397')}}</a>
                </li>
            </ul>
          <div class="tiggle"></div>
          <div class="innertiggle"></div>
      </div>
      </div>
    </div>
   <!--</div>-->
</template>
<script>
  import "../styles/js/jquery-1.8.3.min";
  import Cookies from "js-cookie";
  import { InfiniteScroll, Spinner,Toast,MessageBox } from "mint-ui";
  import { logout } from "../api/api.js";
  export default {
  	name: "Home",
  	props:["reload"],
  	data () {
  		return {
  			sheetVisible1:false,
  			sheetVisible2:false,
  			sheetVisible3:false,
  			actions1: [],
  			actions2: [],
  			actions3: [],
  			tab2:false,
  			tab3:false,
  			tab4:false,
  			menu_child:[],
  			menu_children:[],
  			result1:[],
  			result2:[],
  			result3:[],
  			result4:[],
  			result5:[],
  			num:0,
  			num1:0,
  			num2:0,
  			staff_type:false,
        permission:{},
        visitor:0,
        lang: ""
  		};
  	},

  	created(){
  	  this.lang = localStorage.getItem('locale') || Cookies.get('language') || 'zh';

  		this.menu = JSON.parse(localStorage.getItem("menu"));
  		if(this.menu){
  			for(var i=0;i<this.menu.length;i++){
  				if(this.menu[i].to==="/construction"){
  					this.result1 = this.menu[i].children.map(a => a.to);
  				}
				if(this.menu[i].to==="/hotline"){
  					this.result2 = this.menu[i].children.map(a => a.to);
  				}
				if(this.menu[i].to==="/carryout"){
  					this.result3 = this.menu[i].children.map(a => a.to);
  				}
				if(this.menu[i].to==="/event"){
  					this.result4 = this.menu[i].children.map(a => a.to);
  				}
				if(this.menu[i].to==="/system"){
  					this.result5 = this.menu[i].children.map(a => a.to);
  				}
			}
  		}

  		this.permission = JSON.parse(localStorage.getItem("permission"));
	    for(var key in this.permission){
          if(key==="/event/appointment/fit"){
            if(this.permission[key].indexOf("POST")>-1){
              this.visitor=1;
            }
          }
      }

  		if(localStorage.getItem("contractor_id")){
  			this.staff_type=true;
  		}


  		for(var j=0;j<this.menu_child.length;j++){
  			this.menu_children.push(this.menu_child[j].to);
  		}

  		let body = document.querySelector("body");
		//          let body = document.querySelector('.content');
  		body.addEventListener("click",(e)=>{
  			if(["tab2","son2","tab3","son3","tab4","son4"].indexOf(e.target.id)===-1){
  				this.tab2 = false;
  				this.tab3 = false;
  				this.tab4 = false;
  			}
  		});

  		if(localStorage.getItem("token")){
  			this.ask_num();
  		}

      // var u = navigator.userAgent;
      //   // app = navigator.appVersion;
  		// // console.log(u);
      // var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //g
      // var isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
      // if (isAndroid) {  //这个是安卓操作系统
      //     // 有软键盘时,tabbar隐藏
      //     var win_h = $(window).height();//关键代码
      //     window.addEventListener('resize', function () {
      //         if($(window).height() < win_h){
      //             $('.clearfix').hide();
      //         }else{
      //             $('.clearfix').show();
      //         }
      //     });
      // }

  //     if (isIOS) {
  // 　　　　//这个是ios操作系统
  //     }


  	},

  	watch:{
  		reload:function (newValue) {
  			if(newValue){
  				this.ask_num();
  			}
  		},
  	},


  	methods:{
  		menu_toggle1(){
  		  //回到主页
  		  this.$router.push({path:"/main"});
        //带有刷新整页面效果的跳转
        //this.$router.push({path:"/main",query:{fresh:1}});

        // this.went().then(val => {this.fresh();});
  			// this.$router.push({path:"/main"});
  			// this.$router.go(0);
        // this.$router.go({path:"/main"});
  			this.tab2=false;
  			this.tab3=false;
  			this.tab4=false;
  			// setInterval(() => {
  			//   this.$router.go(0);
  			// }, 200);

  		},

      went(){
  		  this.$router.push({path:"/main"});
  		  return Promise.resolve("ok");
  		  },

      fresh(){
  		  this.$router.go(0);
      },

  		menu_toggle2(){
  			if(this.tab2===true){
  				this.tab2=false;
				//           $('.bt-name2,.bt-name2 .iconfont').css('color','#808080');
				//           $('bt-name2 .sanjiao').css('border-right','5px solid #808080').css('border-bottom','5px solid #808080');
  			}else{
  				this.tab2=true;
  				this.tab3=false;
  				this.tab4=false;
				//           $('.bt-name2,.bt-name2 .iconfont').css('color','rgb(24,144,255)');
				//           $('.bt-name3,.bt-name3 .iconfont').css('color','#808080');
				//           $('.bt-name3,.bt-name3 .iconfont').css('color','#808080');
				//           $('bt-name2 .sanjiao2').css('border-right','5px solid rgb(24,144,255)').css('border-bottom','5px solid rgb(24,144,255)');
  			}
  		},

  		menu_toggle3(){
  			if(this.tab3===true){
  				this.tab3=false;
				//           $('.bt-name3,.bt-name3 .iconfont').css('color','#808080');
  			}else{
  				this.tab3=true;
  				this.tab2=false;
  				this.tab4=false;
				//           $('.bt-name3,.bt-name3 .iconfont').css('color','rgb(24,144,255)');
				//           $('.bt-name2,.bt-name2 .iconfont').css('color','#808080');
				//           $('.bt-name4,.bt-name4 .iconfont').css('color','#808080');
  			}
  		},

  		menu_toggle4(){
  			if(this.tab4===true){
  				this.tab4=false;
				//           $('.bt-name4,.bt-name4 .iconfont').css('color','#808080');
  			}else{
  				this.tab4=true;
  				this.tab3=false;
  				this.tab2=false;
				//           $('.bt-name4,.bt-name4 .iconfont').css('color','rgb(24,144,255)');
				//           $('.bt-name2,.bt-name2 .iconfont').css('color','#808080');
				//           $('.bt-name3,.bt-name3 .iconfont').css('color','#808080');
  			}
  		},

  		new_1(){
  			this.$router.push({path:"/visitor/addvisitors"});
  		},
  		new_2(){
  			this.$router.push({path:"/visitor/cargo"});
  		},
  		new_3(){
  			this.$router.push({path:"/workorder/newwork"});
  		},
  		new_4(){
  			this.$router.push({name:"construct_attention"});
  		},
  		new_5(){
  			this.$router.push({path:"/carryout/newcarryout"});
  		},
  		self_1(){
  			this.$router.push({path:"/system/me"});
  		},
  		self_2(){
  			this.$router.push({path: "/system/changepassword"});
  		},
  		self_3(){
  			this.$router.push({path:"/system/applypermission"});
  		},

      changeLanguage() {
  		  let lang = localStorage.getItem('locale') || Cookies.get('language') || 'zh';

  		  this.$axios({
          method: "POST",
          url: "/v1/system/language/change/",
          data:{
            site_id:localStorage.getItem("site"),
            language:lang==="en"?1:2,
          },
        }).then((res)=>{

  		  });

  		  if (lang == 'zh') {
          Cookies.set('language', 'en')
          Cookies.set('django_language', 'en')
          this.$store.commit("setLocale", 'en');
          this.$i18n.locale = 'en'
          window.location.reload()
        }
        if (lang == 'en') {
          Cookies.set('language', 'zh')
          Cookies.set('django_language', 'zh-Hans')
          this.$store.commit("setLocale", 'zh');
          this.$i18n.locale = 'zh'
          window.location.reload()
        }

      },

  		logout(){
			//        console.log(this.$route);
			//        console.log(window.location.href);
  			MessageBox.confirm(this.$t('tabbar.confirm.is_logout'),{confirmButtonText: this.$t('button.ok'), cancelButtonText: this.$t('button.cancel') }).then(action => {
  				logout({
  					data:{
  						site_id: localStorage.getItem("site"),
  					}
  				}).then((res)=> {
  					if(res.status===200){
  						var site=localStorage.getItem("site");
  						var user=localStorage.getItem("username");
  						let locale = localStorage.getItem("locale");
  						this.$store.commit("LOGOUT");
  						localStorage.clear();
						//              localStorage.setItem('website',this.$route.fullPath);
  						if(user){
  							localStorage.setItem("username",user);
  						}
						  if(site){
  							localStorage.setItem("site",site);
  						}
  						if(locale) {
						    localStorage.setItem("locale", locale)
              }
  						this.$router.push({path: "/login"});
  					}
  				})
  					.catch(function (err) {

  					});
  			});
		},

  		ask_num(){
  			this.$axios({
  				method: "GET",
  				url: "/v1/system/notification/info/",
  				headers: {Authorization: "JWT " + localStorage.getItem("token")},
  				data:{
  					site_id: localStorage.getItem("site"),
  				}
  			}).then((res)=> {
  				if(res.status===200){
  					var toRead=0;
  					for(var i=0;i<res.data.message_list.length;i++){
  						if(res.data.message_list[i].is_read===false){
  							toRead+=1;
  						}
  					}

  					this.num1=toRead;
  					this.num2=res.data.need_handle_list.length;
  					this.num=this.num1+this.num2;

  				}
  			});
  		},
  	},

  	mounted() {
  		this.actions1 = [
  			{
  				name: this.$t("tabbar.b5e4c08c974f11e9a5048c8590a7c397"),
  				method:this.new_4
  			},
  			{
  				name: this.$t("tabbar.b5e4c1a4974f11e9a5048c8590a7c397"),
  				method:this.new_3
  			},
  			{
  				name: this.$t("tabbar.b5e4c28a974f11e9a5048c8590a7c397"),
  				method:this.new_5
  			},
  			{
  				name: this.$t("tabbar.b5e4c370974f11e9a5048c8590a7c397"),
  				method:this.new_1
  			},
  		];

  		this.actions2 = [{
  			name: this.$t("tabbar.b5e4c456974f11e9a5048c8590a7c397")
  		}, {
  			name: this.$t("tabbar.b5e4c58c974f11e9a5048c8590a7c397"),
  		}];

  		this.actions3 = [
  			{
  				name: this.$t("tabbar.b5e4c668974f11e9a5048c8590a7c397"),
  				method:this.self_1
  			}, {
  				name: this.$t("tabbar.b5e4c73a974f11e9a5048c8590a7c397"),
  				method:this.self_2
  			},
  			{
  				name: this.$t("tabbar.b5e4c80c974f11e9a5048c8590a7c397"),
  				method:this.self_3
  			},{
  				name: this.$t("tabbar.b5e4c8de974f11e9a5048c8590a7c397"),
  				method:this.logout
  			}
  		];
  	}

  };
</script>
