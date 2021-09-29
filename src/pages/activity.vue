<template>
  <div class="wrapper">
    <div class="header">
      <div class="headline">{{ headline }}</div>
      <div class="cover">
        <img v-if="cover" :src="cover" alt="封面">
      </div>
    </div>
    <div class="main">
      <div class="recommend-wrapper" v-if="recommend.goods.length">
        <div class="headline">{{ recommend.headline }}</div>
        <div class="goods" v-for="goods in recommend.goods" :key="goods.id">
          <div class="cover">
            <img :src="goods.cover" :alt="goods.name">
          </div>
          <div class="content">
            <div class="headline">{{ goods.headline }}</div>
            <div class="price">￥{{ goods.price }}</div>
            <div class="remark">{{ goods.remark }}</div>
            <div class="btn-convert cover">
              <img src="/assets/images/btn-convert.png" alt="兑换">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'activity',
  data: () => {
    return {
      headline: '',
      cover: '',
      recommend: {
        headline: '',
        goods: []
      }
    };
  },
  mounted() {
    const KEY = 'activity-GuoQin';
    let data$;

    try {
      const data = JSON.parse(sessionStorage.getItem('activity-GuoQin') || void 0);
      data$ = Promise.resolve(data);
    } catch ( error ) {
      data$ = axios.get('/database/activity-GuoQin.json')
                   .then(({ data }) => ( sessionStorage.setItem(KEY, JSON.stringify(data)), data ));
    }

    data$.then(response => {
      const { headline, cover, recommend } = response;
      this.headline = headline;
      this.cover = cover;
      this.recommend = recommend;
    }).catch(error => {
      console.error('[axios] get /database/activity-GuoQin.json', error);
    });

  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.wrapper .header {
  flex: 0 0 auto;
}

.wrapper .main {
  display: flex;
  flex-direction: column;
  flex: 1 1 auto;
  padding: 21px 11px 0;
  background-color: #f2f8e1;
}

.wrapper .cover {
  font-size: 0 !important;
  line-height: 0 !important;
}

.header .headline {
  padding: 24px 11px 21px;
  font-size: 22px;
  font-weight: 400;
  line-height: 28px;
  color: #000;
  background-color: #efefef;
}

.header .cover {
  width: 100%;
  overflow: hidden;
}

.main .recommend-wrapper {
  flex: 1 1 auto;
  padding: 0 10px 70px;
  background-color: #fff;
  border-radius: 20px 20px 3px 3px;
}

.main .recommend-wrapper > div {
  padding: 14px 10px 6px;
  border-bottom: 1px solid #e1cbcb;
}

.main .recommend-wrapper > div.headline {
  color: #000;
  font-size: 15px;
  font-weight: 400;
  line-height: 22px;
  padding-left: 17px;
}

.main .recommend-wrapper > div.goods {
  display: flex;
  flex-direction: row;
  font-size: 18px;
  font-weight: 400;
  line-height: 25px;
}

.goods > .cover {
  flex: 0 0 auto;
  width: 109px;
  height: 109px;
  overflow: hidden;
  margin-right: 4px;
}

.goods > .content {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 100%;
}

.goods .headline {
  font-weight: 300;
  color: #000;
}

.goods .price {
  color: #e42f2f;
}

.goods .remark {
  font-size: 13px;
  color: #6d7278;
  line-height: 18px;
}

.goods .btn-convert {
  position: absolute;
  right: 0;
  bottom: 2px;
  width: 33px;
  height: 33px;
}
</style>
