<style scoped>
  .weui-div{
    width: auto;
    margin: 0 1rem;
    border-radius: 8px;
  }

  .weui-pic{
    position: relative;
    margin-right: 10px;
    float: left;
  }

  .pic{
    height: 50px;
    width: 50px;
    display: block
  }

  .weui-cells{
    margin-top: 0;
  }

  .weui-cell{
    padding: 10px 1rem;
  }

  .weui-photo-browser-modal{
    z-index: 3000;
  }

  .back_color{
    background-color: transparent;
  }
</style><template>
<div class="weui-div" v-if="visible">
    <!--<div class="weui-cells__title attach">{{$t('imgView.b5e1dd54974f11e9a5048c8590a7c397')}}</div>-->

    <div class="weui-cells weui-cells_form back_color">
        <div class="weui-cell">
            <div class="weui-cell__hd weui-pic" v-bind:class="clsn" v-for="(item,index) in imgurls" v-on:click="view(index)" :key="index">
                <img v-bind:src="item" class="pic">
            </div>
        </div>
    </div>
</div>
</template><script>
import "../../node_modules/swiper/dist/css/swiper.css";
import "../../static/lib/jquery-weui.min.js";
import "../../static/lib/swiper.min.js";
export default {
	props: ["urlarr", "clsn", "visible"],
	data() {
		return {

		};
	},

	created(){
		//    console.log(this.clsn)
	},

	computed:{
		imgurls(){
			if(this.urlarr){
				return this.urlarr.split(",").map(function(x) {
					return (x = "/source/" + x);
				});
			}else{
				return [];
			}

		}
	},
	methods: {
		view(index) {
			// console.log(index);
			var items = new Array();
			var clsn = this.clsn;
			// console.log(clsn);

			$("." + clsn + ".weui-cell__hd").each(function() {
				// console.log($(this));
				items.push(
					$(this)
						.children("img")
						.attr("src")
				);
			});

			// console.log(items);

			var imgview = $.photoBrowser({
				items: items,
				initIndex: index,
				onSlideChange: function(index) {

				},

				onOpen: function() {

				},
				onClose: function() {

				}
			});
			imgview.open(index);
		}
	}
};
</script>