<template>
  <div>
   <ul class="mui-table-view mui-table-view-chevron">
				<li class="mui-table-view-cell mui-media" v-for="(item, i) in newsList" :key="i">
					<router-link :to="'/home/newsinfo/' + item.id" class="mui-navigate-right">
						<img class="mui-media-object mui-pull-left" :src="item.img">
						<div class="mui-media-body">
							<h3>{{ item.msg }}</h3>
							<p class='mui-ellipsis'>
                <span>发表时间：{{ item.time | dateFormat('YYYY-MM-DD HH:mm') }}</span>
                <span>点击量：{{ item.clicks }}</span>
              </p>
						</div>
					</router-link>
				</li>
			</ul>
  </div>
</template>

<style lang="scss" scoped>
.mui-table-view{
  li{
    h3{
      font-size: 15px;
    }
    .mui-ellipsis{
        font-size: 12px;
        color: rgb(140, 140, 240);
        display: flex;
        justify-content: space-between;
      }
  }
}
</style>


<script>
export default {
  data() {
    return {
      newsList: []
    }
  },
  created() {
    this.getNewList()
  },
  methods: {
    getNewList () {
      this.$axios.get("/apiNews/data")
      .then(res => {
        let newsArr = res.data.data
        newsArr.forEach(element => {
          let meg = JSON.parse(element.content); 
          // console.log(meg.abstract) 
          // console.log(meg.media_info.avatar_url) 
          this.newsList.push({
            msg:meg.title,
            img:meg.media_info.avatar_url,
            clicks:meg.comment_count,
            time:meg.cursor,
            id:meg.group_id
          })
        });

        console.log(this.newsList); 
      })
      .catch(err =>{
        Toast('获取新闻失败')
      })
    }
  }
};
</script>
