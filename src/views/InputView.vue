<script setup>
import { ref } from 'vue';
import axios from 'axios';

//ページタイトルと入力状態
const pgtitle = ref('データを入力してください');
const resText = ref('未完了');

//入力データのオブジェクト
const post = ref({
    fields:{
    title: {stringValue:''},
    author: {stringValue:''},
    impressions: {stringValue:''},
    date: {stringValue:''},
    },

});

//Firestoreにデータを入力する関数
const postData = async () => {
    const projectId = 'vue-test1-2d4c2'; // ここにあなたのプロジェクトIDを入力してください
    const collectionName = 'books'; // ここにあなたのコレクション名を入力してください
    const url = `https://firestore.googleapis.com/v1/projects/${projectId}/databases/(default)/documents/${collectionName}`; // ここにあなたのドキュメントIDを入力してください
    try {
        const response = await axios.post(url, post.value);
        resText.value = 'データが正常に送信されました。';
        console.log(response.data);
        resText.value = 'データが正常に送信されました。';
    } catch (error) {
        resText.value = 'データの送信に失敗しました。';
        console.error(error);
    }
};

</script>

<template>
  <div class="container">
    <h1>{{ pgtitle }}</h1>
      <div>
        <p>タイトル：<input type="text" id="title" v-model="post.fields.title.stringValue" required /></p>
        <p>著者：<input type="text" id="author" v-model="post.fields.author.stringValue" required /></p>
        <p>感想：<input type="text" id="impressions" v-model="post.fields.impressions.stringValue" required /></p>
        <p>日付：<input type="date" id="date" v-model="post.fields.date.stringValue" required /></p>
      <button type="button" @click="postData">送信</button>
    </div>
  </div>
</template>