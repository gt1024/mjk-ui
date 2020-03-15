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
</style>
<template>
    <div class="file-wrapper">
        <div class="file-mask" v-show="loading">
            <span>上传中...</span>
        </div>
        <div class="file-lists" v-if="fileLists.length > 0">
            <div
                class="file-list"
                v-for="(item, index) of fileLists"
                :key="item.id">
                <a
                    :href="getImageUrl(item.url)+ '?time= '+ new Date().getTime()"
                    :download="item.fileName"
                    :style="{
                        width: noAdd ? '100%' : '85%'
                    }" >
                    {{item.fileName}}
                </a>
                <van-icon
                    @click="deleteFile(index)"
                    slot="right-icon"
                    name="close"
                    class="van-cell__right-icon"
                    v-if="!noAdd" />
            </div>
          <span v-if="agent === 'Android'" style="display: inline-block;color:rgb(190,196,204);font-size:12px;padding: 0 10px;">
            安卓部分机型无法打开excel附件，如需下载或浏览，请到网页端操作。</span>
        </div>

        <div class="file-btn" style="text-align:center" v-if="!noAdd">
            点击上传
            <input
                type="file"
                @change="uploadFile"
                :accept="this.accept" />
        </div>

    </div>
</template>

<script>
import { cloneDeep } from 'lodash'
// import common from '@/utils/common'
import { Icon, Toast } from 'vant'
import axios from 'axios'
// import { userAgent } from "@/utils/common.js";
export default {
    data() {
        return {
            loading: false,
            agent: ''
        }
    },
    computed: {
        fileLists: {
            get() {
                return this.defaultFileLists
            },
            set() {}
        }
    },
    props: {
        url: {
            type: String,
            required: true
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
      let u = navigator.userAgent;
      console.log(this.userAgent(u))
      this.agent = this.userAgent(u)
    },
    methods: {
        userAgent(u) {

            let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1;   //判断是否是 android终端
            let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);     //判断是否是 iOS终端

            if(isAndroid){
            return 'Android';
            }else if(isIOS){
            return 'IOS';
            }else{
            return 'PC';
            }
        },
        // getImageUrl: common.getImageUrl,
        getImageUrl: '',
        deleteFile(index) {
            const { fileLists } = this
            fileLists.splice(index, 1)
            this.fileLists = cloneDeep(fileLists)
            this.$emit('change', fileLists)
        },
        uploadFile(e) {
            console.log(file);
            const formData = new FormData();
            const { files } = e.target
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

            this.loading = true;

            instance.post(this.url, formData, config).then(res => {
                this.loading = false;
                const { content, errNo, error } = res.data;
                if (errNo === 1) {
                    Toast({message: error})
                    return
                }

                if (content) {
                    const { fileLists } = this
                    const url = content.results.url
                    fileLists.push({
                        id: url,
                        url,
                        fileName
                    })
                    this.fileLists = cloneDeep(fileLists)
                    this.$emit('change', fileLists)
                }
            })
        },
    },
    components: {
        'van-icon': Icon
    }
}
</script>
