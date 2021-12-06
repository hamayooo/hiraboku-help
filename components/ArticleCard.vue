<template>
  <article class="Article">
    <NuxtLink :to="`/article/${article.slug}`" class="Article_Link">
      <h3 class="Article_Title">{{article.title}}</h3>
      <p class="Article_Description">{{article.description}}</p>
    </NuxtLink>
  </article>
</template>

<script>
import { formatDate } from 'utils/date'

export default {
  props: {
    article: {
      type: Object,
      default: null
    }
  },
  computed: {
    authorName() {
      return (this.article.author && this.article.author.fullName) || 'NO NAME'
    }
  },
  methods: {
    formatDate(dateStr) {
      return dateStr ? formatDate(dateStr) : ''
    }
  }
}
</script>

<style scoped>
.Article {
  border: 1px solid #e5e5e5;
  box-shadow: 0 2px 2px 0 rgba(0,0,0,.05);
  border-radius: 4px;
  margin: 0 0 16px 0;
  display: flex;
  flex-direction: column;
}
.Article_Link {
  flex: 1 auto;
  padding: 16px;
  line-height: 1.5;
  color: #333;
  text-decoration: none;
  border-radius: 4px;
  transition: background .2s;
  background: #fff;
}
.Article_Link:hover {
  background: #f8f8f8;
}
.Article_Link:active {
  background: none;
  transition: none;
}
.Article_Title {
  font-size: 1.4rem;
  margin: 0 0 4px 0;
  padding: 0;
}
.Article_Description {
  font-size: 1.2rem;
  margin: 0;
  padding: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
@media (min-width: 960px) {
  .Article {
    width: calc(50% - 10px);
    margin: 0 16px 16px 0;
  }
  .Article:nth-child(2n) {
    margin: 0 0 16px 0;
  }
}
</style>