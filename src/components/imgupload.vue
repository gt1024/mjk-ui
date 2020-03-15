<style scoped>
  .mint-msgbox-wrapper {
    z-index: 3000 !important;
  }

  .loader_text{
    padding-left: 1rem;
    margin-bottom: 3px;
    display: block;
    color: #aab2bd;
    font-size: 12px;
  }

  .weui-cell{
    padding: 10px 15px;
  }

  .weui-cell:before{
    border-top:none
  }

  .weui-uploader__input-box{
    width: 50px;
    height: 50px;
  }

  .weui-uploader__input-box:before{
    height: 30px;
  }

  .weui-uploader__input-box:after{
    width: 30px;
  }

  .weui-cells:before{
    border-top: none;
  }
</style><template>
  <div>
    <div ref="uploader" class="weui-cells weui-cells_form uploader_selector" :id="selector" style="margin-top: 0;border-radius: 8px;">

      <div class="weui-cell">
        <div class="weui-cell__bd">
          <div class="weui-uploader">
            <div class="weui-uploader__bd">
              <ul class="weui-uploader__files" id="img_upload" ref="uploadercontainer" @click="handleClickUploadList"></ul>
              <div class="weui-uploader__input-box">
                <input name="file" class="weui-uploader__input" type="file" accept="image/*" multiple="">
              </div>
            </div>
          </div>
        </div>
      </div>
      <span class="loader_text">{{$t('imgupload.b5e21b0c974f11e9a5048c8590a7c397')}}:{{$t('app.component.imgupload.imgsize')}}</span>
      <!--<span class="loader_text">{{$t('imgupload.b5e21b0c974f11e9a5048c8590a7c397')}}:图片格式:jpg/jpeg/png/gif/bmp;{{$t('imgupload.b5e21ddc974f11e9a5048c8590a7c397')}}:小于3M</span>-->
    </div>
    <!-- 图片预览 -->
    <div class="weui-gallery">
      <span class="weui-gallery__img"></span>
      <div class="weui-gallery__opr">
        <a href="javascript:" class="weui-gallery__del">
          <i class="weui-icon-delete weui-icon_gallery-delete"></i>
        </a>
      </div>
    </div>
  </div>
</template><script>
import "../styles/weui.min.css";
import weui from "weui.js";
import { Toast, MessageBox,Indicator } from "mint-ui";

export default {
	props: ["url", "selector", "uploaded"],
	data() {
		return {
			uploadList: [],
			successImgList: [],
			pic_source: ""
		};
	},


	methods: {
		handleClickUploadList(e) {
			var that = this;
			var target = e.target;
			while (!target.classList.contains("weui-uploader__file") && target) {
				target = target.parentNode;
			}
			if (!target) return;

			var url = target.getAttribute("style") || "";
			var id = target.getAttribute("data-id");

			if (url) {
				url = url.match(/url\((.*?)\)/)[1].replace(/"/g, "");
			}
			var gallery = weui.gallery(url, {
				className: "custom-name",
				onDelete: function onDelete() {
					MessageBox.confirm(that.$t("imgupload.b5e2188c974f11e9a5048c8590a7c397"),{confirmButtonText: that.$t('button.ok'), cancelButtonText: that.$t('button.cancel') }).then(action => {
						//            for (let i = 0, len = that.uploadList.length; i < len; ++i) {
						//              var file = that.uploadList[i];
						//              if (file.id + "" == id) {
						//                that.uploadList.splice(i, 1);
						//                break;
						//              }
						//            }

						for (let i = 0, len = that.successImgList.length; i < len; ++i) {
							var item = that.successImgList[i];
							// console.log(item);
							// console.log(that.uploadList);
							if (item.id + "" == id) {
								that.uploadList.splice(i, 1);
								that.successImgList.splice(i, 1);
								break;
							}
						}
						//发给父组件
						that.pic_source = that.successImgList
							.map(function(x) {
								return x.url;
							})
							.join();
						that.$emit("updatepics", that.pic_source);

						target.remove();
						gallery.hide();
					});
				}
			});
		},

		clear() {
			this.pic_source = "";
			this.uploadList = [];
			this.successImgList = [];
			let el = this.$refs.uploadercontainer;
			el.innerHTML = "";
			this.$emit("updatepics", this.pic_source);
		},

		inituploaded() {
			if (this.uploaded) {
				let el = this.$refs.uploadercontainer;
				let sources = this.uploaded.split(",");
				for (var i = 0; i < sources.length; i++) {
					var uploaded_pics = $(
						"<li class=\"weui-uploader__file\" data-id=\"" +
              (i + 18) +
              "\" style=\"background-image:url(/source/" +
              sources[i] +
              ")\"></li>"
					);
					$(el).append(uploaded_pics);
					var img = {
						id: i + 18,
						url: sources[i]
					};
					this.successImgList.push(img);
					this.uploadList.push(i + 18);
				}

				//发给父组件
				this.pic_source = this.successImgList
					.map(function(x) {
						return x.url;
					})
					.join();
				this.$emit("updatepics", this.pic_source);
			}else{

			}
		}
	},

	mounted() {

		this.inituploaded();
		var that = this;
		let el = that.$refs.uploader;
		let id = "#" + that.selector;
		weui.uploader(id, {
			url: that.url,
			auto: true,
			type: "file",
			fileVal: "file",
			compress: {
				width: 400,
				height: 400,
				quality: 0.8
			},
			onBeforeQueued: function(files) {
				// Indicator.open();
				if (
					[
						"image/jpg",
						"image/jpeg",
						"image/png",
						"image/gif",
						"image/bmp"
					].indexOf(this.type) < 0
				) {
					Toast("不支持的图片格式，图片格式仅限JPG、PNG、GIF");
					// Indicator.close();
					return false;
				}
				if (this.size > 10 * 1024 * 1024) {
					Toast(this.$t("imgupload.b5e21990974f11e9a5048c8590a7c397"));
					// Indicator.close();
					return false;
				}

				if (that.uploadList.length > 4) {
				  // Indicator.close();
					Toast(this.$t("imgupload.b5e21a4e974f11e9a5048c8590a7c397"));
					// Indicator.open();
					return false;
				}
			},

			onQueued: function() {
				that.uploadList.push(this.id); // 如果是base64上传，file.base64可以获得文件的base64
			},

			onBeforeSend: function(data, headers) {
				if(localStorage.getItem("token")) {
					$.extend(headers, {
						Authorization: "JWT " + localStorage.getItem("token")
					});
				}
				if(localStorage.getItem("token")) {
					$.extend(data, { site_id: localStorage.getItem("site") });
				}
			},
			onProgress: function(procent) {},
			onSuccess: function(ret) {
				// Indicator.close();
        // console.log(ret);
				var img = {
					id: this.id,
					url: ret.content.results.url
				};
				that.successImgList.push(img);
				//        console.log(img)
				//发给父组件
				that.pic_source = that.successImgList.map(
					function(x) {
						return x.url;
					}).join();
				// console.log(that.pic_source);
				that.$emit("updatepics", that.pic_source);
			},

			onError: function(err) {
			  // console.log(err)
				// Indicator.close();
			}
		});
	},

	watch:{
		uploaded:function (newValue,oldValue) {
			// console.log(newValue);
			this.inituploaded();
		}
	}
};
</script>
