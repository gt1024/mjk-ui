<style scoped>
  .file-btn,
  .file-list {
    position: relative;
    padding: 10px 15px;
    line-height: 24px;
    background: #fff;
    color: #333;
    font-size: 14px;
    overflow: hidden;
  }

  .file-btn input {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
  }

  .file-list a {
    float: left;
    width: 85%;
    white-space: nowrap;
    word-break: break-all;
    word-wrap: break-word;
    text-overflow: ellipsis;
    overflow: hidden;
  }

  .file-list i {
    float: right;
  }

  .file-mask {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba(255, 255, 255, 0.7);
    z-index: 999999;
  }

  .file-mask span {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .vancell .file-list {
    min-height: 3.4rem;
    line-height: 3.4rem;
    padding: 0 1rem;
  }

  .van-uploader__preview-image {
    display: block;
    width: 80px;
    height: 80px;
  }

  .van-image {
    position: relative;
    display: inline-block;
  }

  .van-image__img, .van-image__error, .van-image__loading {
    display: block;
    width: 100%;
    height: 100%;
  }

  .van-uploader__preview {
    position: relative;
    margin: 0 8px 8px 0;
  }

  .van-uploader__wrapper {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-wrap: wrap;
    flex-wrap: wrap;
  }

  .van-image.van-uploader__preview-image.hide, .hide.van-uploader__preview {
    display: none;
  }

  .van-uploader__preview-delete {
    position: absolute;
    top: -8px;
    right: -8px;
    color: #969799;
    font-size: 18px;
    background-color: #fff;
    border-radius: 100%;
  }

  .van-uploader__wrapper {
    margin-top: 10px;
  }
</style>
<template>
  <div class="file-wrapper">

    <div class="file-lists" v-if="fileLists.length > 0">
      <div
        v-if="fileLists.length > 0"
        class="file-list"
      >
        <div class="van-uploader__wrapper imglist">
          <div :class="[{hide: getType(item)},'van-uploader__preview']" v-for="(item, index) of fileLists"
               :key="item.id">
            <div class="van-image van-uploader__preview-image" :class="{ hide: getType(item) }"
                 style="overflow: hidden; border-radius: 4px;" @click="isShow">
              <img v-if="getType(item) == false" :src="getImageUrl(item.url)+ '?time= '+ new Date().getTime()" alt=""
                   class="van-image__img" style="object-fit: cover;">
            </div>
            <i v-if="getType(item) == false && !noAdd" @click="deleteFile(item.index)"
               class="van-icon van-icon-clear van-uploader__preview-delete"><!----></i>
          </div>
        </div>

        <div class="filelist" v-for="(item, index) of fileLists"
             :key="item.id">

          <a
            v-if="getType(item) == true"
            :href="getImageUrl(item.url)+ '?time= '+ new Date().getTime()"
            :download="item.fileName"
            :style="{
                        width: noAdd ? '100%' : '85%'
                    }">
            {{item.fileName}}
          </a>
          <van-icon
            @click="deleteFile(item.index)"
            slot="right-icon"
            name="clear"
            class="van-cell__right-icon"
            v-if="getType(item) == true && !noAdd"/>
        </div>

      </div>
      <span v-if="agent === 'Android'"
            style="display: inline-block;color:rgb(190,196,204);font-size:12px;padding: 0 10px;">
            安卓部分机型无法打开excel附件，如需下载或浏览，请到网页端操作。</span>
    </div>

    <div class="file-btn" style="text-align:center" v-if="!noAdd">
      点击上传
      <input
        type="file"
        ref="uploadref"
        @change="uploadFile"
        :accept="this.accept"/>
    </div>

    <!--<van-image-preview-->
    <!--v-model="show"-->
    <!--:images="images"-->
    <!--@change="onChange"-->
    <!--&gt;-->
    <!--&lt;!&ndash;<template v-slot:index>第{{ index }}页</template>&ndash;&gt;-->
    <!--</van-image-preview>-->

  </div>
</template>

<script>
  import {cloneDeep} from 'lodash'
  // import common from '@/utils/common'
  import {Icon, Toast, ImagePreview} from 'vant'
  import axios from 'axios'
  // import {userAgent} from "@/utils/common.js";
  // import {Indicator} from "mint-ui";
  export default {
    data() {
      return {
        loading: false,
        agent: '',
        show: false,
        index: 0
      }
    },
    computed: {
      fileLists: {
        get() {
          return this.defaultFileLists
        },
        set() {
        }
      }
    },
    props: {
      url: {
        type: String,
        required: false
      },
      defaultFileLists: {
        type: Array,
        default: () => []
      },
      accept: String,
      limit: {
        type: Number,
        default: 15
      },
      noAdd: {
        type: Boolean,
        default: false
      }
    },
    created() {
      // let u = navigator.userAgent;
      // console.log(userAgent(u))
      // this.agent = userAgent(u)
    },
    methods: {
      isShow(){
        if (!this.noAdd) {
          return
        }
        let images = []
        this.fileLists.filter(d => d && d.url.indexOf('.png') > -1 || d && d.url.indexOf('.jpg') > -1 || d && d.url.indexOf('.jpeg') > -1).map(d => {
          images.push(this.getImageUrl(d.url))
        })
        ImagePreview({
          images,
          startPosition: 0,
          showIndex: false,
          onClose() {
            // do something
          }
        });
      },
      onChange(index) {
        this.index = index;
      },
      getType(d){
        if (d && d.url.indexOf('.xlsx') > -1) {
          return true
        } else {
          return false
        }
      },

      // getImageUrl: this.getImageUrl,
      getImageUrl(path) {
        let baseUrl = 'https://wx.mjk24.com/source/'
        return baseUrl + path;  //123服务器
        //return "https://wx.mjk24.com/source/" + path;  //123服务器
        //return "https://efms.mjk24.com/source/" + path;  //腾讯服务器
      },
      deleteFile(index) {
        const {fileLists} = this
        //console.log(fileLists.findIndex((n) => n.index == index))
        let i = fileLists.findIndex((n) => n.index == index)
        fileLists.splice(i, 1)
        this.fileLists = fileLists;
        this.$emit('change', fileLists)
      },
      uploadFile(e) {
        const formData = new FormData();
        const {files} = e.target
        const file = files[0]
        const fileName = file.name
        const fileSize = files[0].size

        if (fileSize / 1024 / 1024 > this.limit) {
          Toast({message: `附件大小不超过${this.limit}M`})
          return
        }

        formData.append("site_id", localStorage.getItem("site"))
        formData.append("file", file)

        const config = {
          headers: {
            Authorization: "JWT " + localStorage.getItem("token")
          }
        }
        const instance = axios.create({
          withCredentials: true
        })

        //this.loading = true;
        // Indicator.open()
        instance.post(this.url, formData, config).then(res => {
          //this.loading = false;
          // Indicator.close();
          const {content, errNo, error} = res.data;
          if (errNo === 1) {
            Toast({message: error})
            return
          }

          if (content) {
            const {fileLists} = this
            const url = content.results.url
            fileLists.push({
              id: url,
              url,
              fileName
            })
            let temp = fileLists.map((d, idx) => {
              d.index = idx
            })
            this.fileLists = cloneDeep(temp)
            this.$emit('change', fileLists)
          }
        })
        this.$refs.uploadref.value=null;
      },
    },
    components: {
      'van-icon': Icon,
      "van-image-preview": ImagePreview
    }
  }
</script>
