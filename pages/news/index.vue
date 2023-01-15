<template>
  <div class="d-flex-column justify-content-between">
    <h1>News</h1>
    <p class="mt-3 d-flex-1">Current Page: {{ currentPage }}</p>

    <b-container class="p-0">
      <b-row v-for="news of paginatedNews" :key="news.id" no-gutters class="mt-4">
        <b-col>
          <b-card type="button" :title="news.title" :class="{ 'border-primary': news.id === hoveredNews }" @mouseover.stop="mouseoverNews(news.id)" @mouseleave="mouseleaveNews(news.id)" @click.prevent="openNews(news.id)">
            <b-card-text>

            </b-card-text>
            <template #footer>
              <div class="d-flex-column">
                <small class="text-muted">{{ news.score }} point</small>
                <small class="text-muted">by {{ news.by }}</small>
                <small class="text-muted">{{ new Date(new Date() - new Date(news.time)).toLocaleDateString() }}</small>
              </div>

            </template>
          </b-card>
        </b-col>

      </b-row>
    </b-container>

    <b-pagination v-model="currentPage" :per-page="perPage" :total-rows="newsList.length" align="fill" class="mt-2"></b-pagination>
    <b-button class="mt-4" variant="outline-primary" @click="updateNewsList">Обновить</b-button>

  </div>
</template>

<script>


export default {
  async asyncData({ $axios }) {
    const newsId = await $axios.$get('https://hacker-news.firebaseio.com/v0/newstories.json');
    newsId.splice(0, 400);

    const newsList = [];

    await Promise.all(newsId.map(async (id) => {
      const newNews = await $axios.$get((`https://hacker-news.firebaseio.com/v0/item/${id}.json`));
      newsList.push(newNews);
    }));

    return { newsList }
  },
  data() {
    return {
      newsList: [],
      perPage: 4,
      currentPage: 1,
      hoveredNews: null,
    }
  },
  computed: {
    paginatedNews() {
      const { currentPage, perPage } = this;
      const start = (currentPage - 1) * perPage;
      const end = currentPage * perPage;

      return this.newsList.slice(start, end);
    },
  },
  methods: {
    openNews(news) {
      this.$router.push('/news/' + news)
    },
    mouseoverNews(newsId) {
      this.hoveredNews = newsId;
    },
    mouseleaveNews(newsId) {
      this.hoveredNews = null;
    },
    async updateNewsList() {
      await this.$nuxt.refresh();
    },
  },
};
</script>
