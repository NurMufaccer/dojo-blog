<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input v-model="title" type="text" required />
      <label>Content:</label>
      <textarea v-model="body" required></textarea>
      <label>Tags (hit enter to add tag):</label>
      <input v-model="tag" type="text" @keydown.enter.prevent="handleKeydown" />
      <div v-for="tag in tags" :key="tag" class="pill">#{{ tag }}</div>
      <button>Add Post</button>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { useRouter } from "vue-router";

export default {
  setup() {
    const title = ref("");
    const body = ref("");
    const tag = ref("");
    const tags = ref([]);

    const router = useRouter();

    const handleKeydown = () => {
      if (!tags.value.includes(tag.value)) {
        tag.value = tag.value.replace(/\s/, "");
        tags.value.push(tag.value);
      }
      tag.value = "";
    };

    const handleSubmit = async (params) => {
      const post = {
        title: title.value,
        body: body.value,
        tags: tags.value,
      };
      await fetch("http://localhost:3000/posts", {
        method: "POST",
        headers: { "Content-type": "application.json" },
        body: JSON.stringify(post),
      });

      router.push({ name: "home" });
    };

    return { title, body, tag, tags, handleKeydown, handleSubmit };
  },
};
</script>

<style>
form {
  max-width: 488px;
  margin: 0 auto;
  text-align: left;
}
input,
textarea {
  display: block;
  margin: 10px 0;
  box-sizing: border-box;
  width: 100%;
  padding: 10px;
  border: 1px solid rgb(207, 207, 207);
}
textarea {
  height: 160px;
}
label {
  display: inline-block;
  position: relative;
  margin-top: 30px;
  margin-bottom: 10px;
  font-size: 20px;
  color: white;
}
label::before {
  content: "";
  height: 100%;
  width: 100%;
  display: block;
  background: #ff8800;
  position: absolute;
  z-index: -1;
  padding-right: 40px;
  left: -30px;
  transform: rotateZ(-1.5deg);
}
button {
  display: block;
  margin-top: 40px;
  background: #ff8800;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 16px;
}
.pill {
  display: inline-block;
  margin: 10px 10px 0 0;
  color: #444;
  background: #ddd;
  padding: 8px;
  border-radius: 20px;
  font-size: 14px;
}
</style>
