<template>
  <section>
    <b-card class="mt-4" :title="news.title"
      :sub-title="(`${news.score} point by ${news.by} ${new Date(news.time * 1000)}`)">
    </b-card>
    <hr>
    <h3>Комментарии</h3>
    <news-comments v-for="comment in comments" :key="comment.id" :comment="comment" />
  </section>
</template>



<script>


import NewsComments from '@/components/NewsComments.vue';

export default {
  components: {
    NewsComments,
  },
  async asyncData({ $axios, params }) {
    const news = await $axios.$get(`https://hacker-news.firebaseio.com/v0/item/${params.id}.json`);
    const newsKids = news.kids;

    const comments = [];
    newsKids.forEach(async (comment) => {
      comments.push(await $axios.$get(`https://hacker-news.firebaseio.com/v0/item/${comment}.json`));
    });

    return { news, comments }
  },
  data() {
    return {
      news: [],
    }
  },
  methods: {
    async getComment({ $axios, comment }) {
      return await $axios.$get(`https://hacker-news.firebaseio.com/v0/item/${comment}.json`)
    }
  }
}
</script>
