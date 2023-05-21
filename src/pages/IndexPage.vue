<template>
  <div class="index-page">
    <a-input-search
        v-model:value="searchParams.text"
        placeholder="input search text"
        enter-button="Search"
        size="large"
        @search="onSearch"
    />
    {{ JSON.stringify(searchParams) }}
    <my-divider/>
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="text" tab="文章">
        <PostList/>
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片" force-render>
        <PictureList/>
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UserList/>
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import {ref, watchEffect} from "vue";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/UserList.vue";
import PictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";
import {useRoute, useRouter} from "vue-router";

const router = useRouter();
const route = useRoute();
const activeKey = route.params.category;

const initSearchParam = {
  text: '',
  pageSize: 10,
  pageNum: 1,
}

const searchParams = ref(initSearchParam);

watchEffect(() => {
  searchParams.value = {
    ...initSearchParam,
    text: route.query.text,
  } as any;
});

const onSearch = (value: string) => {
  alert(value)
  router.push({
    query: searchParams.value
  })
}
const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value
  })
}
</script>
