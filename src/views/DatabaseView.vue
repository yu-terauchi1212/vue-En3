<script setup>
import { ref , onMounted } from 'vue';
import axios from 'axios';

//ページタイトルと入力状態
const pgtitle = ref('読書データベース');
const db = ref([]);

const projectId = 'vue-test1-2d4c2'; // ここにあなたのプロジェクトIDを入力してください
    const collectionName = 'books'; // ここにあなたのコレクション名を入力してください
    const url = `https://firestore.googleapis.com/v1/projects/${projectId}/databases/(default)/documents/${collectionName}`;

    // データ取得（一覧表示用）
const getData = async () => {
  try {

    const res = await axios.get(url);
    db.value = res.data.documents || [];// res.data.documents: 各ドキュメントのフルパスやフィールドを含む
  } catch (err) {
    console.error('データ取得エラー:', err);
  }
}

onMounted(getData);



</script>

<template>

    <h1>著者を検索すると今まで登録してきた本の一覧が表示されます。</h1><br>
    <div class="container">
        <input type="text" v-model="db.fields.author.stringValue" placeholder="著者名を入力" />
        <button @click="getData(doc.name)">検索</button>

        <p>ドキュメントがありません。</p>
    </div>
</template>