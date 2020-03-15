<style>

</style><template>
	<div>
    <div class="g_sousuo">
      <input type="text" :placeholder='$t("search_new.b5e2df6a974f11e9a5048c8590a7c397")' v-model="search"/>
      <a href="#" class="am-icon-search"></a>
    </div>
  </div>
</template><script>
  export default {
  props: [],
	data() {
		return {
  			search:"",
  			param_object:{},
  			pageList:[],
  		};
	},
  	created:function () {
  		this.debouncedGetAnswer=_.debounce(this.searchList, 800);
  	},

  watch:{
  		search:function(newValue,oldValue) {
  			this.debouncedGetAnswer();
  		}
  },

  methods: {
  		//    查询
  		searchList:function(){
			//      this.param_object={
			//        site_id:localStorage.getItem('site'),
			//        search:this.search
			//      };
			//      if(this.status){
			//        this.param_object.filter=JSON.stringify({status:this.status})
			//      }

			//	    this.$axios({
			//          method: "GET",
			//          url:this.url,
			//          headers: {Authorization: 'JWT ' + localStorage.getItem('token')},
			//          params:this.param_object,
			////          params: {
			////            site_id:localStorage.getItem('site'),
			////            filter:JSON.stringify({status:this.status}),
			////            search:this.search
			////        },
			//			}).then((res)=> {
			//      if (res.status === 200) {
			//        this.pageList = res.data.results;
			//        this.$emit("results", this.pageList);
			//        this.$emit("text", this.search);
			//      } else {
			//        Toast(this.$t('search_new.b5e2e910974f11e9a5048c8590a7c397'))
			//      }
			//        });
			//      this.loadingTextBtn = false;
			//	    this.isShow = false;
  			this.$emit("text", this.search);
  		},
  }
  };
</script>