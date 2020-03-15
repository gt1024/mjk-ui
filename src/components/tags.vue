<style scoped>
   .el-tag + .el-tag {
      margin-left: 10px;
  }
  .button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
  }
  .input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
  }
  .el-tag{
    width: 20% !important;
    margin-top: 14px;
  }
</style><template>
  <div>
      <el-tag
      v-for="tag in this.tag_arr.tags"
      :key="tag.id"
      :color="tag.style"
      closable
      @close="handleClose(tag)">
        {{tag.name}}</el-tag>

    <!--<el-input-->
      <!--class="input-new-tag"-->
      <!--v-if="inputVisible"-->
      <!--v-model="inputValue"-->
      <!--ref="saveTagInput"-->
      <!--size="small"-->
      <!--@keyup.enter.native="handleInputConfirm"-->
      <!--@blur="handleInputConfirm"-->
    <!--&gt;-->
    <!--</el-input>-->

    <!--<el-button v-else class="button-new-tag" size="small" @click="showInput">-->
      <!--+ New Tag-->
    <!--</el-button>-->
  </div>
</template><script>
   import Vue from "vue";
   import Tag from "element-ui";
   import "element-ui/lib/theme-chalk/index.css";
   Vue.use(Tag);
   export default {
   	props: ["tag_arr","content"],
   	data() {
   		return {
			//        dynamicTags: [this.$t('tags.b5e31778974f11e9a5048c8590a7c397'), this.$t('tags.b5e3182c974f11e9a5048c8590a7c397'), this.$t('tags.b5e318ae974f11e9a5048c8590a7c397')],
   			inputVisible: false,
   			inputValue: "",
   			tags_arrays:[],
   		};
   	},

   	components: {
   		Tag
	  },

   	created(){
   		this.$emit("tag_result",this.tag_arr);
		//       console.log(this.content);
		//       for(var i=0;i<this.content.length;i++){
		//         this.tags_arrays.push(this.content[i].tags);
		//         console.log(this.tags_arrays)
		//       }
   	},

   	methods: {
   		handleClose(tag) {
   			this.tag_arr.tags.splice(this.tag_arr.tags.indexOf(tag), 1);
   		},

   		showInput() {
   			this.inputVisible = true;
   			this.$nextTick(_ => {
   				this.$refs.saveTagInput.$refs.input.focus();
   			});
   		},

   		handleInputConfirm() {
   			let inputValue = this.inputValue;
   			if (inputValue) {
   				this.dynamicTags.push(inputValue);
   			}
   			this.inputVisible = false;
   			this.inputValue = "";
   		}
   	},

   	updated(){
   		this.$emit("tag_result",this.tag_arr);
   	},
};
</script>