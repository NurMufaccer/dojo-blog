<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="postWithTag" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<script>
import Spinner from "../components/Spinner.vue";
import PostList from "@/components/PostList.vue";
import getPosts from "@/composables/getPosts";
import { computed } from "vue";
import { useRoute } from "vue-router";
import TagCloud from "@/components/TagCloud.vue";
export default {
  components: { Spinner, PostList, TagCloud },
  setup() {
    const route = useRoute();
    const { error, posts, load } = getPosts();

    load();
    const postWithTag = computed(() => {
      return posts.value.filter((p) => p.tags.includes(route.params.tag));
    });
    return { error, posts, postWithTag };
  },
};
</script>

<style>
.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
</style>
