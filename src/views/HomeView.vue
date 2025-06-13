<script setup>
import { ref , onMounted } from 'vue';
import axios from 'axios';

//ページタイトルと入力状態
const pgtitle = ref('読書データベース');
const db = ref([]);
const randomBook = ref([]);

const projectId = 'vue-test1-2d4c2'; // ここにあなたのプロジェクトIDを入力してください
const collectionName = 'books'; // ここにあなたのコレクション名を入力してください
const url = `https://firestore.googleapis.com/v1/projects/${projectId}/databases/(default)/documents/${collectionName}`;

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
  <main>
    <div class="container">
      <h1>読書管理DB</h1>
      <p>このアプリは、読書の記録を管理するためのものです。</p>
      <p>本の情報を入力し、リストで確認できます。</p><br>

      <p>History…過去のデータをランダムで表示します。</p>
      <p>→</p>
    </div>
  </main>
</template>
