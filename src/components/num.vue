<style>


</style><template>
    <div class="mui-numbox" data-numbox-step='10' data-numbox-min='10' data-numbox-max='90'>
        <button class="mui-btn mui-btn-numbox-minus" type="button">-</button>
        <input class="mui-input-numbox" type="number" />
        <button class="mui-btn mui-btn-numbox-plus" type="button">+</button>
    </div>
</template><script>
export default {
	data() {
		return {
			result: 0,
		};
	},
	methods: {
		minus() {
			this.result--;
			this.$emit("input", { res: this.result, other: "--" });
		},
		plus() {
			this.result++;
			this.$emit("input", { res: this.result, other: "++" });
		}
	}
};

</script>