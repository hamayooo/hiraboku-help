<template>
  <main class="Container">
    <article v-if="article" class="Article">
      <div class="Article_Header">
        <ul class="Breadcrumb">
          <li class="Breadcrumb_Item">
            <nuxt-link to="/" class="Breadcrumb_Link">Home</nuxt-link>
          </li>
          <li v-if="category" class="Breadcrumb_Item">
            <nuxt-link :to="`/category/${category.slug}`" class="Breadcrumb_Link">{{category.name}}</nuxt-link>
          </li>
        </ul>
        <h1 class="Article_Title">{{article.title}}</h1>
      </div>
      <!-- eslint-disable-next-line vue/no-v-html -->
      <div class="Article_Body" v-html="article.body"></div>
      <!-- <Like /> -->
      <section v-if="relatedArticles.length > 0" class="Related">
        <h2 class="Related_Heading">Related articles</h2>
        <ul class="Related_List">
          <li v-for="relatedArticle in relatedArticles" :key="relatedArticle._id" class="Related_Item">
            <NuxtLink :to="`/article/${relatedArticle.slug}`">{{relatedArticle.title}}</NuxtLink>
          </li>
        </ul>
      </section>
    </article>
  </main>
</template>

<script>
import { getArticleBySlug, getArticles } from 'api/article'
import { formatDate } from 'utils/date'

export default {
  layout: 'sub',
  async asyncData({ $config, params }) {
    const article = await getArticleBySlug($config, params.slug)
    const { articles } = await getArticles($config, {
      query: {
        tags: article.tags,
        slug: {
          ne: article.slug
        },
        select: ['slug', 'title']
      },
    })
    return {
      article,
      category: article.category,
      relatedArticles: articles,
    }
  },
  computed: {
    publishDate() {
      return this.article._sys.createdAt ? formatDate(this.article._sys.createdAt) : ''
    },
    publishDateForAttr() {
      return this.publishDate.replace(/\//g, '-')
    },
    authorName() {
      return (this.article.author && this.article.author.fullName) || 'NO NAME'
    },
    authorSelfIntroduction() {
      return (this.article.author && this.article.author.introduction && this.article.author.introduction.html) || ''
    }
  }
}
</script>

<style scoped>
.Breadcrumb {
  display: flex;
  align-items: center;
  min-width: 0;
  margin: 8px 0;
  padding: 0;
  flex-wrap: wrap;
}
.Breadcrumb_Item {
  margin: 0;
  padding: 0;
  list-style: none;
}
.Breadcrumb_Link {
  padding: 0;
  margin: 0 32px 0 0;
  display: block;
  font-size: 1.4rem;
  position: relative;
  text-decoration: none;
  line-height: 1.5;
}
.Breadcrumb_Link:hover {
  text-decoration: underline;
}
.Breadcrumb_Link::before {
  pointer-events: none;
  position: absolute;
  content: "";
  right: -18px;
  top: 7px;
  width: 6px;
  height: 6px;
  border-top: 1px solid #888;
  border-right: 1px solid #888;
  transform: rotate(45deg);
}
.Breadcrumb_Item:last-child .Breadcrumb_Link {
  margin: 0;
}
.Breadcrumb_Item:last-child .Breadcrumb_Link::before {
  content: none;
}
.Article {
  margin: 0 auto;
  padding: 24px 24px 40px 24px;
}
.Article_Header {
  margin: 0 0 24px 0;  
}
.Article_Title {
  font-size: 2.4rem;
  line-height: 1.5;
  font-weight: bold;
  margin: 0;
  padding: 0;
}
.Article_Body >>> p {
  margin: 0 0 24px 0;
}
.Article_Body >>> img {
  max-width: 100%;
  height: auto;
  margin: 32px auto;
  display: block;
}
.Related {
  border-top: 1px solid #e5e5e5;
  margin: 36px 0 0 0;
  padding: 28px 0 0 0;
}
.Related_Heading {
  margin: 0 0 16px 0;
  padding: 0;
  font-size: 1.8rem;
}
.Related_List {
  margin: 0;
  padding: 0;
}
.Related_Item {
  margin: 0 0 8px 28px;
  padding: 0;
}


@media (min-width: 600px) {
  .Article {
    max-width: 700px;
    padding: 60px;
  }
  .Article_Header {
    margin: 0 0 48px 0;  
  }
  .Related {
    margin: 56px 0 0 0;
    padding: 44px 0 0 0;
  }
}
</style>