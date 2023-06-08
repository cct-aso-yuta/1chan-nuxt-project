<template>
  <link
    rel="stylesheet"
    href="https://use.fontawesome.com/releases/v5.6.4/css/all.css"
  />
  <Header />
  <!-- 会話履歴 -->
  <div v-for="(_comment, index) in comments">
    <div class="comment">
      <p>
        <span> {{ index + 1 }} </span> {{ _comment.name }}
        <div> <!-- v-if="comment.name === user.name" -->
          <!-- <button @click="toggleIsModalOpen">編集</button> -->
          <button @click="updateAction(_comment)">編集</button>
          <button @click="deleteAction(_comment)">削除</button>
        </div>
      </p>
      <p>{{ _comment.time }}</p>
      <p>{{ _comment.content }}</p>
      <p id="favoposi">
        <input
          type="button"
          value="&#xf164;"
          class="fas"
          id="favo"
          @click="sumLikeNum(_comment.id)"
        />
        いいね{{ _comment.like_num }}
      </p>
    </div>
  </div>
  <!-- コメント投稿 -->
  <div>
    <p>名前: <input v-model="comment.name" type="text"></p>
    <p>本文: <input v-model="comment.content" type="textarea"></p>
    <input type="button" value="送信" @click="createAction">
  </div>
  <!-- top page -->
  <div id="page_top"><a href="#"></a></div>
  <!-- <Modal v-if="isModalOpen" /> -->
  
</template>

<style>
.comment {
  background-color: skyblue;
  padding: 20px;
  margin: 30px;
  border: solid;
}

#page_top {
  width: 90px;
  height: 90px;
  position: fixed;
  right: 0;
  bottom: 0;
  background: #ef3f98;
  opacity: 0.6;
  border-radius: 50%;
}
#page_top a {
  position: relative;
  display: block;
  width: 90px;
  height: 90px;
  text-decoration: none;
}
#page_top a::before {
  font-family: "Font Awesome 5 Free";
  font-weight: 900;
  content: "\f102";
  font-size: 25px;
  color: #fff;
  position: absolute;
  width: 25px;
  height: 25px;
  top: -40px;
  bottom: 0;
  right: 0;
  left: 0;
  margin: auto;
  text-align: center;
}
#page_top a::after {
  content: "PAGE TOP";
  font-size: 13px;
  color: #fff;
  position: absolute;
  top: 45px;
  bottom: 0;
  right: 0;
  left: 0;
  margin: auto;
  text-align: center;
}


#favo {
  transform: scale(0.9);
  background-color: transparent;
  border-color: transparent;
  color: blue;
  font-size: 20px;
  transition: all 0.5s ease 0s;
}

#favo:hover {
  transform: scale(1.3);
}

#favoposi {
  margin: 0em 0;
  display: block;
  text-align: right;
  transform: translateY(-80px);
}
</style>

<script setup>
import Header from '~/components/Header.vue';
// import Modal from '~/components/Modal.vue';

let comments = ref(null);

let comment = ref({
  id: null,
  name: "",
  content: "",
  time: "",
  like_num: 0,
});

const readAction = async () => {
  const { data, error } = await useFetch("http://localhost:8080/comment/view", {
    method: "GET",
  });
  comments.value = data._rawValue;
  console.log(comments)
};

const createAction = async () => {
  const { data, error } = await useFetch("http://localhost:8080/comment/create", {
    method: "POST",
    body: { ...comment.value },
  });
  comment.value.name = "";
  comment.value.content = "";
  readAction()
};

const deleteAction = async (id) => {
  const {data, error} = await useFetch  ("http://localhost:8080/comment/delete/"+id,
    {
      method: "DELETE",
    }
  )
  readAction()
};

const sumLikeNum = async (id) => {
  comment.value = comments.value.find((comment) => comment.id === id);

  const { data, error } = await useFetch(
    "http://localhost:8080/comment/likeNumUpdate/" + id,

    {
      method: "PUT",

      body: { ...comment.value },
    }
  );
  readAllAction()
};

const router = useRouter();
const updateAction = async (id) => {
  router.push(id);
  // router.push("/modi/" + id); //授業のファイル構成
};


let isModalOpen = ref(false);
console.log(isModalOpen)

const toggleIsModalOpen = () => {
  isModalOpen.value = !isModalOpen.value;
  console.log(!isModalOpen)
}

onMounted(readAction(),{method:"GET"})

</script>
