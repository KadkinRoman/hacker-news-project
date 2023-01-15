<template>
  <section>
    <b-card class="mt-4" :title="news.title" :sub-title="subTitle">
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
    const newsKids = news.kids || [];

    const comments = [];
    newsKids.forEach(async (comment) => {
      const newComment = await $axios.$get(`https://hacker-news.firebaseio.com/v0/item/${comment}.json`);
      comments.push(newComment);
    });

    return { news, comments }
  },
  computed: {
    subTitle() {
      const subTitleBody = `${this.news.score} point by ${this.news.by} ${new Date(this.news.time * 1000)}`
      return subTitleBody;
    }
  }
}
</script>
