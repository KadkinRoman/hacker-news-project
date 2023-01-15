<template>

  <b-card>
    <p class="card-text">{{ comment.text }}</p>
    <b-button v-if="comment.kids !== undefined" v-b-toggle="`item-${comment.id}`" variant="primary" size="sm" @click="getComments(comment.kids)">
      <b-icon-arrow-down></b-icon-arrow-down>
    </b-button>
    <b-collapse :id="`item-${comment.id}`" class="mt-2">
      <news-comments v-for="commentKid in commentsKids" :key="commentKid.id" :comment="commentKid" />
    </b-collapse>
  </b-card>

</template>

<script>
import { BIconArrowDown } from 'bootstrap-vue';

export default {
  components: { BIconArrowDown, },
  props: {
    comment: {
      type: Object,
      default: function() {
        return {}
      },
      required: false
    }
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
          const result = await this.$axios.$get(`https://hacker-news.firebaseio.com/v0/item/${commentId}.json`);
          this.commentsKids.push(result);
        });
      }
    }
  },

}
</script>
