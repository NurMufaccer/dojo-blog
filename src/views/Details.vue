<template>
  <div>
    <div v-if="error">{{ error }}</div>
    <div v-if="post" class="post">
        <h3>{{ post.title }}</h3>
        <p class="pre">{{ post.body }}</p>
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<script>
import getSinglePost from '../composables/getSinglePost';
import Spinner from '../components/Spinner.vue';
import { useRoute } from 'vue-router'

export default {
    components: { Spinner },
    props: ['id'],
  setup(props) {
    const route = useRoute()
        console.log(route);
        
        const { post, error, load } = getSinglePost(route.params.id)

        load()
        return { post, error }
    }
}
</script>

<style>
.post{
  max-width: 1200px;
  margin: 0 auto;
}
.post p {
  color: #444;
  line-height: 1.5en;
  margin-top: 20px;
}
.pre{
  white-space: pre-wrap;
}
</style>