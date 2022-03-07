<template>
  <main class="Container">
    <div class="Inner">
      <div class="Category_Header">
        <em v-if="category.emoji && category.emoji.value">{{
          category.emoji.value
        }}</em>
        <div class="Category_Text">
          <h2>{{ category.name }}</h2>
          <p>{{ category.description }}</p>
        </div>
      </div>
      <div class="Articles">
        <article v-for="article in articles" :key="article._id" class="Article">
          <NuxtLink :to="`/article/${article.slug}`" class="Article_Link">
            <h3 class="Article_Title">{{ article.title }}</h3>
            <p class="Article_Description">{{ htmlToText(article.body) }}</p>
          </NuxtLink>
        </article>
        <Pagination
          :total="total"
          :current="1"
          :base-path="`/category/${category.slug}`"
        />
      </div>
    </div>
  </main>
</template>

<script>
import { mapGetters } from 'vuex'
import { getSiteName } from 'utils/head'
import { htmlToText } from 'html-to-text'

export default {
  async asyncData({ $config, store, params }) {
    await store.dispatch('fetchApp', $config)
    await store.dispatch('fetchCategories', $config)

    const category = store.getters.categories.find(
      (_category) => _category.slug === params.slug
    )
    await store.dispatch('fetchArticles', {
      ...$config,
      category: (category && category._id) || '',
    })

    return {
      category,
    }
  },
  head() {
    return {
      title: getSiteName(this.app),
    }
  },
  computed: {
    ...mapGetters(['app', 'articles', 'total']),
  },
  methods: {
    htmlToText(html) {
      return htmlToText(html, {
        selectors: [
          {
            selector: 'img',
            format: 'skip',
          },
        ],
      })
    },
  },
}
</script>

<style scoped>
.Inner {
  margin: 0 auto;
  padding: 24px 24px 40px 24px;
}
.Category_Header {
  display: flex;
  align-items: center;
  margin: 0 0 30px 0;
}
.Category_Header > em {
  font-size: 4rem;
  min-width: 64px;
  font-style: normal;
  line-height: 1;
}
.Category_Text > h2 {
  margin: 0;
  padding: 0;
  font-size: 1.6rem;
}
.Category_Text > p {
  margin: 0;
  padding: 0;
  font-size: 1.4rem;
  line-height: 1.5;
}
.Article {
  border: 1px solid #e5e5e5;
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.05);
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
  transition: background 0.2s;
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
@media (min-width: 600px) {
  .Inner {
    padding: 60px;
    max-width: 700px;
  }
  .Category_Header {
    margin: 0 0 40px 0;
  }
}
</style>
