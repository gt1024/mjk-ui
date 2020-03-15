<template>
  <div id="app">
    <img src="@/assets/logo.png">
    <h1>{{ msg }}</h1>
    <div class="pb48 underline">
      <h2>Button组件</h2>
      <p>1.基础样式：type</p>
      <mjk-button>default</mjk-button>
      <mjk-button type="success">success</mjk-button>
      <mjk-button type="error">error</mjk-button>
      <mjk-button type="warning">warning</mjk-button>
      <mjk-button type="info" style="min-width:120px">info</mjk-button>
      <p>2.按钮大小：size</p>
      <mjk-button>default</mjk-button>
      <mjk-button size="small">small</mjk-button>
      <mjk-button size="medium">medium</mjk-button>
      <mjk-button size="large">large</mjk-button>
      <p>3.按钮形状：shape</p>
      <mjk-button>default</mjk-button>
      <mjk-button shape="circle" type="info">circle</mjk-button>
      <mjk-button shape="rectangle" type="info">rectangle</mjk-button>
      <p>4.禁用按钮：isDisabled</p>
      <mjk-button isDisabled>default</mjk-button>
      <mjk-button type="success" isDisabled>success</mjk-button>
      <mjk-button type="error" isDisabled>error</mjk-button>
      <mjk-button type="warning" isDisabled>warning</mjk-button>
      <mjk-button type="info" isDisabled>info</mjk-button>
    </div>
    <div class="pt48">
      <h2>modal组件</h2>
      <p>1.基础样式</p>
      <mjk-button @click="show1=true">default</mjk-button>
      <mjk-modal 
        @on-ok="show1=false"
        @on-cancel="show1=false"
        :isShow="show1"></mjk-modal>
      <p>2.自定义内容</p>
      <mjk-button @click="show2=true">自定义内容</mjk-button>
      <mjk-button @click="show3=true">自定义宽度</mjk-button>
      <mjk-modal 
        @on-ok="show2=false"
        @on-cancel="show2=false"
        :isShow="show2">
        <template>自定义文字</template>
        <template slot="modal-title">
          <p>Hello World!</p>
        </template>
        <template slot="modal-body">
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
          <p>test</p>
        </template>
        <template slot="modal-footer">
          <mjk-button type="warning" @click="show2=false">取消</mjk-button>
        </template>
      </mjk-modal>
      <mjk-modal 
        @on-ok="show3=false"
        @on-cancel="show3=false"
        :width="300"
        :isShow="show3"></mjk-modal>
      <p>3.自定义size</p>
      <mjk-button @click="showSize('small')">small</mjk-button>
      <mjk-button @click="showSize('medium')">medium</mjk-button>
      <mjk-button @click="showSize('large')">large</mjk-button>
      <mjk-button @click="showSize('full')">full</mjk-button>
      <mjk-modal 
        @on-ok="show4=false"
        @on-cancel="show4=false"
        :size="size"
        :isShow="show4"></mjk-modal>
    </div>

    <div class="pt48">
      <h2>upload组件</h2>
      <p>1.example</p>
      <mjk-upload
        url=''
        v-if="source"
        :defaultFileLists="
          source ?
            source.split(',').map(s => {
              let fileName
              const fileNameArr = /\/([^/]+)$/.exec(s)

              if (fileNameArr) {
                fileName = fileNameArr[1]
              }

              return {
                id: s,
                url: s,
                fileName
              }
            }) : []
        " noAdd />
        <p>2.basic</p>
        <mjk-upload
          @change="handleFileChange"
          url='/v1/upload/member_list/'
          accept='image/*,.xlsx,.xls'
       />
      <!-- <mjk-modal 
        @on-ok="show1=false"
        @on-cancel="show1=false"
        :isShow="show1"></mjk-modal>
      <p>2.自定义内容</p>
      <mjk-button @click="show2=true">自定义内容</mjk-button>
      <mjk-button @click="show3=true">自定义宽度</mjk-button>
      <mjk-modal 
        @on-ok="show3=false"
        @on-cancel="show3=false"
        :width="300"
        :isShow="show3"></mjk-modal>
      <p>3.自定义size</p>
      <mjk-button @click="showSize('small')">small</mjk-button>
      <mjk-button @click="showSize('medium')">medium</mjk-button>
      <mjk-button @click="showSize('large')">large</mjk-button>
      <mjk-button @click="showSize('full')">full</mjk-button>
      <mjk-modal 
        @on-ok="show4=false"
        @on-cancel="show4=false"
        :size="size"
        :isShow="show4"></mjk-modal> -->
    </div>

    <div class="pt48">
      <h2>暂无数据组件</h2>
      <p>1.example</p>
      <mjk-nodata v-if="pageList.length === 0"/>

    </div>
  </div>
</template>

<script>
import Button from '@/components/Button/Button'
import Modal from '@/components/Modal/Modal'
import upload from '@/components/upload.vue'
import NoData from '@/components/NoData.vue'
export default {
  name: 'app',
  components: {
    mjkButton: Button,
    mjkModal: Modal,
    mjkUpload: upload,
    mjkNodata: NoData
    
  },
  data () {
    return {
      msg: 'Welcome to mjk-ui',
      show1: false,
      show2: false,
      show3: false,
      show4: false,
      size: '',
      source: '/suzhou/2019/1/14/logo/8b477e5b66d63e62b8e77731f1a84cf6.png',
      pageList: []
    }
  },
  methods: {
    showSize(item) {
      this.size = item
      this.show4 = true
    },
    handleFileChange(files) {
        this.source = files.map(f => f.url).join(',')
      },
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding-bottom: 200px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.pt48, .py48 {
  padding-top: 48px;
}
.pb48, py48 {
  padding-bottom: 48px;
}
.underline {border-bottom: 1px solid #ccc;}
</style>
