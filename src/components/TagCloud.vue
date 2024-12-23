<template>
  <div class="tagCloud">
    <h3>Tag</h3>
    <div v-for="tag in tags" :key="tag">
      <router-link :to="{ name: 'tag', params: { tag: tag } }">
        #{{ tag }}
      </router-link>
    </div>
  </div>
</template>

<script>
import useTags from "../composables/useTags";
export default {
  props: ["posts"],
  setup(props) {
    const { tags } = useTags(props.posts);

    return { tags };
  },
};
</script>

<style>
.tagCloud {
  padding: 10px;
}
.tagCloud h3 {
  border-bottom: 1px solid #eee;
  padding: 16px 8px;
  color: #444;
}
.tagCloud div {
  display: inline-block;
  padding: 10px;
}
.tagCloud a {
  color: #ccc;
  text-decoration: none;
}
.tagCloud a.router-link-active {
  color: #ff8800;
  font-weight: bold;
}
</style>
