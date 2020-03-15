<style scoped>
.list-zxh .van-field__body {
    height: 100%;
}
.list-zxh .van-list {
  padding: 0 1rem;
}
.list-zxh .van-search {
  padding: 7px 1rem;
}
</style>

<template>
    <div class="list-zxh">
      <van-search
        v-model="searchValue"
        :placeholder='$t("MyList.b60eb766974f11e9a5048c8590a7c397")'
        show-action
        @search="searchData"
      >
        <div slot="action">
          <span @click="searchData">{{$t('MyList.b60eb900974f11e9a5048c8590a7c397')}}</span>
          <span @click="clearSearch" v-if="searchValue">{{$t('MyList.b60eb964974f11e9a5048c8590a7c397')}}</span>
        </div>
      </van-search>
      <van-list
        @load="fetchMoreData"
        :finished="finished"
        :offset="0"
        :immediate-check="false">
        <slot name="cards"></slot>
      </van-list>
    </div>
</template>

<script>
import axios from "axios";
import { Search, List } from "vant";
// import { toFetch } from "./_utils";

export default {
	data() {
		return {
			searchValue: "",
			currentPage: 1,
            pageSize: 20,
			loading: false,
			finished: false,
			results: [],
      totalPages: 0,
		};
	},

	props: {
		urlConfig: Object
	},

	watch:{
		results(value) {
			if (value.length < 20 || value.length >= this.count) {
				this.finished = true;
			} else {
				this.finished = false;
			}
		}
	},

	methods: {
	  toFetch(config) {
        config.headers = {
          "Authorization": "JWT " + localStorage.getItem("token")
        };

        const { method, data } = config;

        if (method === "get" || method === "delete") {
          config.params = {
            site_id: localStorage.getItem("site"),
            ...data
          };
        } else {
          config.data = {
            site_id: localStorage.getItem("site"),
            ...data
          };
        }

        return axios(config);
    },

		clearSearch() {
			this.searchValue = "";
			this.fetchData();
		},

		searchData() {
			this.currentPage = 1;
			this.fetchData();
		},

		fetchMoreData() {
		    // console.log(this.count);
		    this.totalPages = Math.ceil(this.count / this.pageSize);
        const { currentPage } = this;
        this.currentPage = currentPage + 1;

        if(parseInt(this.currentPage) > 1 && parseInt(this.currentPage) <= parseInt(this.totalPages)){
          this.fetchData("add");
        }
		},

		fetchData(type) {
			const { urlConfig, currentPage, pageSize, searchValue } = this;
			// console.log(currentPage);
      // console.log(type);
			urlConfig.data = {
				...urlConfig.data,
				page:currentPage,
				num: pageSize,
				search: searchValue
			};

			this.loading = true;

			this.toFetch(urlConfig).then(res => {
				const { results, count } = res.data;
				this.loading = false;
				this.count = count;

				if (Array.isArray(results)) {
					let temp = [];

					if (type === "add") {
						temp = this.results;
					}

					results.forEach(r => {
						temp.push(r);
					});

					this.results = temp;
					this.$emit("update-lists", temp);
				}
			});
		}
	},

	mounted() {
		this.fetchData();
	},

	components: {
		"van-search": Search,
		"van-list": List,
	},

  created(){

  }

  
};
</script>
