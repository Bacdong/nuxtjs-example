<template>
  <a-layout>
    <Header />

    <a-layout-content class="mt-large">
      <div class="container py-primary">
        <a-breadcrumb>
          <a-breadcrumb-item>
            <NuxtLink to="/">Home</NuxtLink>
          </a-breadcrumb-item>
          <a-breadcrumb-item>Articles</a-breadcrumb-item>
        </a-breadcrumb>
      </div>

      <div class="container">
        <a-list 
          item-layout="vertical" 
          size="large" 
          :pagination="pagination" 
          :data-source="articles"
        >
          <a-list-item slot="renderItem" key="item.title" slot-scope="item">
            <template v-for="{ type, text } in actions" slot="actions">
              <span :key="type">
                <a-icon :type="type" style="margin-right: 8px" />
                {{ text }}
              </span>
            </template>
            <img
              slot="extra"
              width="272"
              :alt="item.title"
              :src="item.image"
              :style="{
                objectFit: 'contain',
                objectPosition: 'center',
                borderRadius: '4px',
                maxHeight: '240px'
              }"
            />
            <a-list-item-meta :description="item.brief">
              <NuxtLink 
                slot="title"
                :to="`/articles/${item.slug}-a${item.id}`"
              >
                {{ item.title }}
              </NuxtLink>
              <a-avatar 
                slot="avatar" 
                :src="'https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png'" 
              />
            </a-list-item-meta>
            {{ item.title }}
            {{ item.brief }}
          </a-list-item>
        </a-list>
      </div>
    </a-layout-content>

    <Footer />
  </a-layout>
</template>

<script>
  const apiUrl = 'https://lavisdecor.art/api/front'
  const limit = 8

  export default {
    watch: {
      '$route.query': '$fetch'
    },

    data: () => ({
      articles: [],
      pagination: {},
      actions: [
        { type: 'star-o', text: '156' },
        { type: 'like-o', text: '156' },
        { type: 'message', text: '2' },
      ],
    }),

    async fetch() {
      const url = `${apiUrl}/story/`

      await this.$axios.$get(url, { 
        params: {
          ...this.$route.query,
          limit: limit
        }
      })
        .then((res) => {
          console.log(`ApiResponse: `, res);
          if (!res.success) {
            this.showMessage(res.error_message)
            return
          }

          this.articles = res.data.results
          this.pagination = {
            'pageSize': res.data.page_size,
            'total': res.data.total,
            'current': parseInt(this.$route.query.page) ?? res.data.current_page,
            'onChange': (page) => {
              this.$router.push({
                path: this.$router.path,
                query: { page: page },
              })
            }
          }
        })
        .catch((e) => {
          this.showMessage(e)
        })
    },

    methods: {
      showMessage(message) {
        this.$notification.open({
          message: 'Notification',
          description: message,
        });
      },
    },
  };
</script>

<style>
  :root {
    --spacing: 40px;
  }

  .container {
    max-width: 1200px;
    margin: 0 auto;
  }

  .p-primary {
    padding: var(--spacing);
  }

  .py-primary {
    padding-top: var(--spacing);
    padding-bottom: var(--spacing);
  }

  .px-primary {
    padding-left: var(--spacing);
    padding-right: var(--spacing);
  }

  .mt-large {
    margin-top: calc(var(--spacing) * 2);
  }

  .my-primary {
    margin-top: var(--spacing);
    margin-bottom: var(--spacing);
  }
</style>
