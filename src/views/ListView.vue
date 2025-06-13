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
//ドキュメント削除
const deleteDoc = async (fullName) => {
  // fullName 例: "projects/…/documents/sample1/ABC123"
  const deleteUrl = `https://firestore.googleapis.com/v1/${fullName}`;

  try {
    await axios.delete(deleteUrl);
    // 削除後、一覧をリロード
    await getData();
  } catch (err) {
    console.error('削除エラー:', err);
  }
}

onMounted(getData);

</script>

<template>
  <div class="container">
    <h1>{{ pgtitle }}</h1>
    <ul v-if="db.length">
      <li v-for="doc in db" :key="doc.name">
        <!-- 表示したいフィールド -->
        {{ doc.fields.title.stringValue }} - {{ doc.fields.author.stringValue }} - {{ doc.fields.impressions.stringValue }} - {{ doc.fields.date.stringValue }}
        <!-- 削除ボタン -->
        <button @click="deleteDoc(doc.name)" style="margin-left:8px">
          削除
        </button>
      </li>
    </ul>
    <p v-else>ドキュメントがありません。</p>
  </div>
</template>
