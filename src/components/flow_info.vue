<style scoped>
  .mint-popup{
    background-color: #fafafa;
  }

  /*.check_span{*/
    /*float: right;*/
    /*width: 20%;*/
    /*color: #0e90d2;*/
  /*}*/
  /*.check_span>i{*/
    /*color:black;*/
  /*}*/
  .flow_style{
    overflow-y: auto;
    height: 95%
  }

   .close_mt{
    width: 90%;
    height: 4rem;
    text-align: center;
    margin: 0 auto;
  }

  .flow_div{
    margin-top: 2rem;
  }

  .close_ul{
    border: none;
  }

  .weui-div{
    margin: 0 !important;
  }
  .weui-cells{
    background-color: #fafafa;
  }
</style><template>
  <div>
    <div class="g1_info" v-if="this.edit||this.next">
			<i class="am-icon-send-o"></i>&nbsp;{{$t('flow_info.b5e3dfaa974f11e9a5048c8590a7c397')}}</div>

     <div class="mz_list" v-if="this.edit||this.next">
			  <ul class="mz_list_m">
          <li>
            <font>{{$t('flow_info.b5e3e31a974f11e9a5048c8590a7c397')}}</font>
            <span class="check_span" @click="popupVisible4 = true">{{$t('flow_info.b5e3e5b8974f11e9a5048c8590a7c397')}}<i class="am-icon-angle-right"></i></span>
          </li>
        </ul>
    </div>

    <mt-popup v-model="popupVisible4" position="right" class="mint-popup-3" :modal="false">
      <div class="flow_style">
        <div class="g1_info">
         <i class="am-icon-send-o"></i>&nbsp;{{$t('flow_info.b5e3d73a974f11e9a5048c8590a7c397')}}</div>

        <div class="g_jindu">
          <div class="g_jinduline"></div>
          <div class="g_jinducon">

            <ul class="g_jinduactive" v-for="(flow,index) in this.edit" :key="index">
              <li>
                {{flow.operation_name}}&nbsp;&nbsp;&nbsp;{{flow.status}}
                <i class="iBlue"></i>
              </li>
              <p>{{flow.operation_time}}</p>
              <p>{{flow.remarks}}</p>
            </ul>

            <ul class="g_jinduactive close_ul" v-if="this.close">
              <li>
                {{close.closed_user}}&nbsp;&nbsp;&nbsp;{{$t('flow_info.b5e3ce02974f11e9a5048c8590a7c397')}}<i class="iBlue"></i>
              </li>
              <p>{{close.closed_time}}</p>
              <p>{{close.remarks}}</p>
              <img-view :urlarr="close.source" clsn="img1" :visible="close.source?true:false"></img-view>
            </ul>

             <ul class="g_jinduactive" v-for="(item,index) in this.next" :key="index">
               <li>
                 {{$t('system.placeholder6')}} {{item.type}} {{item.name}} {{$t('system.placeholder7')}}
                 <i class="iGray"></i>
               </li>
            </ul>
          </div>
        </div>

        <div class="carry_sub" >
          <button class="d_btn" style="width: 100%" @click="popupVisible4 = false">{{$t('flow_info.b5e3e806974f11e9a5048c8590a7c397')}}</button>
          <!--<mt-button @click.native="popupVisible4 = false" size="large" type="primary" class="close_mt">{{$t('flow_info.b5e3e806974f11e9a5048c8590a7c397')}}</mt-button>-->
        </div>
      </div>
    </mt-popup>
  </div>
</template>
<script>
  import imgView from "./imgView.vue";
  export default {
  	props:["edit","next","old","close"],
  	data() {
  		return {
  			popupVisible4:false,
  			close_show:false
  		};
  	},

  	created(){
  		console.log(this.next);
  	},

  	components: {imgView},
  };

</script>
