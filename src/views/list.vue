<template>
  <div class="list">
    <listHeader></listHeader>
    <mt-swipe :auto="3000" class="list_swipe" v-if="this.$route.query.id==0">
      <mt-swipe-item v-for="(e,i) of list.swipe" :key="i">
        <img :src="e" class="w-100" />
      </mt-swipe-item>
    </mt-swipe>
    <img :src="image" class="w-100" v-else />
    <div class="list_title px-10">{{list.title}}</div>
    <div class="list_price px-10">单瓶价：&yen;{{list.price}}</div>
    <div class="between">
      <div class="list_liulanliang px-10">浏览量： {{list.liulanliang}}</div>
      <div class="list_kucun px-10">库存： {{list.isOnSale==1?'有货':'无货'}}</div>
    </div>
  </div>
</template>

<script>
import listHeader from "../components/list_header";
export default {
  components: {
    listHeader
  },
  data() {
    return {
      image: this.$route.query.image,
      list: [],
      like: []
    };
  },
  mounted() {
    this.axios.get("/list").then(res => {
      this.list = res.data.results[0];
      this.list.swipe = this.list.swipe.slice(1, -1).split(",");
      this.list.xiangqing = this.list.xiangqing.slice(1, -1).split(",");
      this.list.comment = this.list.comment.slice(1, -1).split(",");
    });
    this.axios.get("like").then(res => {
      this.like = res.data.results;
    });
  }
};
</script>
<style scoped>
.between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.px-10 {
  padding-left: 10px;
  padding-right: 10px;
}
.list {
  padding-top: 7vh;
  text-align: left;
}
.list .list_swipe {
  height: 100vw;
  width: 100%;
}
.list .list_title {
  font-size: 0.9rem;
  line-height: 1.3;
}
.list .list_price {
  font-size: 1.5em;
  color: #f22;
  margin-top: 20px;
  margin-bottom: 10px;
}
.list .list_liulanliang {
  font-size: 0.7em;
  color: #aaa;
}
.list .list_kucun {
  font-size: 0.7em;
  color: #aaa;
}
</style>