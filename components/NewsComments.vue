<template>

  <b-card>
    <p class="card-text">{{ comment.text }}</p>
    <b-button @click="getComments(comment.kids)" v-if="comment.kids !== undefined" v-b-toggle="`item-${comment.id}`" variant="primary" size="sm">
      <b-icon-arrow-down></b-icon-arrow-down>
    </b-button>
    <b-collapse :id="`item-${comment.id}`" class="mt-2">
      <news-comments v-for="comment in commentsKids" :key="comment.id" :comment="comment" />
    </b-collapse>
  </b-card>

</template>

<script>
import { BIconArrowDown } from 'bootstrap-vue';
import Vue from 'vue'
import $axios from 'axios'
Vue.use($axios)

export default {
  components: { BIconArrowDown, },
  props: {
    comment: Object
  },
  data() {
    return {
      commentsKids: []
    }
  },
  methods: {
    getComments(commentsKidsId) {
      if (commentsKidsId !== undefined) {


        commentsKidsId.forEach(async (commentId) => {
          const result = await $axios.get(`https://hacker-news.firebaseio.com/v0/item/${commentId}.json`);
          this.commentsKids.push( result.data );
        });

        console.log('commentsKids', this.commentsKids);
      }
    }
  },

}
</script>
