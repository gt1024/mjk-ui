<style>

</style>
<template>
  <div>
    <ul>
        <li class="am-panel">
              <a data-am-collapse="{parent: '#collapase-nav-1', target: '#user-nav'}">
                  {{$t("app.component.constructor.construction_person")}}<i class="am-icon-angle-right am-fr am-margin-right"></i>
              </a>
              <div class="yy_fxc am-list am-collapse admin-sidebar-sub" id="user-nav">
                <ul class="dowebok"  v-for="(staff,index) in staff_content">
                    <li>
                       <input :value=staff.id v-model="staff_assess" type="checkbox" class=":labelauty" :id=staff.id style="display: none;">
                       <label :for=staff.id>
                         <span class=":labelauty-unchecked-image"></span>
                         <span class=":labelauty-unchecked">{{staff.name}}</span>
                         <span class=":labelauty-checked-image"></span>
                         <span class=":labelauty-checked">{{staff.name}}</span>
                       </label>
                    </li>
                </ul>
              </div>
         </li>
      </ul>
  </div>
</template>
<script>
  export default {
  	props: ["type"],
  	data() {
  		return {
  			staff_content:[],
  			staff_assess:[],
  		};},

  	created(){
  		this.$axios({
  			method: "GET",
  			url: "/v1/construction/staff/worker/",
  			params:{
  				site_id:localStorage.getItem("site"),
  				search_type:this.type,
  				project_id:this.$route.query.mid
  			},
  		}).then((res)=>{
  			// console.log(res);
  			if(res.status===200){
  				this.staff_content=res.data;
  			}
  		});
  	}

  };
</script>
