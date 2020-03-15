<template>
    <div class="nodata">
      <img src="../assets/nodata.png" height="212" width="235" alt="">
       <p style="margin: 0;font-family: Arial;font-size: 18px;color: #888888;">暂无搜索结果</p>
      <div class="she_beizhu she_fujian">
        <button type="button" class="d_btn" style="margin:2rem 0" @click="back">返回</button>
      </div>
    </div>
</template>
<script>
export default {
	methods: {
		back() {
      this.$router.back(-1);
		},
	}
};
</script>
