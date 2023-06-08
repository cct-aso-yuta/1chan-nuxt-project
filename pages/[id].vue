<template>
  <div id="modal_container">
    <div></div>
    <div>
      <p>名前: <input v-model="comment.name" type="text" /></p>
      <p>本文: <input v-model="comment.content" type="textarea" /></p>
      <input type="button" value="送信" @click="updateAction" />
    </div>
  </div>
</template>

<script setup>
let comment = ref({
  id: null,
  name: "",
  content: "",
  time: "",
  // likeNum: "",
});

//   更新前の情報取得
const route = useRoute();
const { id } = route.params;
onMounted(async () => {
  const { data, error } = await useFetch(
    `http://localhost:8080/comment/view/${id}`,
    {
      method: "GET",
    }
  );
  comment.value = data._rawValue;
});

//アップデート
const updateAction = async () => {
  const { data, error } = await useFetch(
    "http://localhost:8080/comment/update/" + id,
    {
      method: "PUT",
      body: comment,
    }
  );
  const router = useRouter();
  router.push("/index");
};
</script>
