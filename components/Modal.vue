<template>
  <div id="modal_container">
    <div id="modal-bg"></div>
    <!-- モーダル本体 -->
    <div id="modal">
      <p>名前: <input v-model="comment.name" type="text" /></p>
      <p>本文: <input v-model="comment.content" type="textarea" /></p>
      <input type="button" value="送信" @click="dateAction" />
    </div>
  </div>
</template>

<style>
#modal-bg {
  position: fixed; /* ブラウザの定位置に固定 */
  background: rgba(0, 0, 0, 0.5); /* 背景色を半透明の黒色に */
  width: 100%; /* 要素の横幅を画面全体に */
  height: 100%; /* 要素の高さを画面全体に */
  top: 0; /* 要素の固定位置をブラウザ最上部に合わせる */
  left: 0; /* 要素の固定位置をブラウザ左側に合わせる */
  z-index: 1000; /* 要素をコンテンツより前面に（要調整） */
}
</style>

<script setup>
let comment = ref({
  id: null,
  name: "",
  content: "",
  time: "",
  LikeNum: 0,
});

//更新前の情報取得
// const route = useRoute();
// const { id } = route.params;
// onMounted(async () => {
//   const { data, error } = await useFetch(
//     `http://localhost:8080/comment/view/${id}`,
//     {
//       method: "GET",
//     }
//   );
//   comment.value = data._rawValue;
// });

//アップデート
const updateAction = async () => {
  const { data, error } = await useFetch(
    "http://localhost:8080/comment/update/" + id,
    {
      method: "PUT",
      body: comment,
    }
  );
  // toggleIsModalOpen()
};
</script>
