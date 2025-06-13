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
        db.value = (res.data.documents || []).map(doc => ({
            name: doc.name,
            fields: doc.fields,
            editTitle: doc.fields.title.stringValue,
            editAuthor: doc.fields.author.stringValue,
            editImpressions: doc.fields.impressions.stringValue,
            editDate: doc.fields.date.stringValue || '',
        
        }))
    } catch (err) {
        console.error('データ取得エラー:', err);
    }
};

const updataDoc = async (doc) => {
    const url = `https://firestore.googleapis.com/v1/${doc.name}`+ '?updateMask.fieldPaths=title&updateMask.fieldPaths=author&updateMask.fieldPaths=impressions&updateMask.fieldPaths=date';
    const body = {
        fields: {
            title: { stringValue: doc.editTitle },
            author: { stringValue: doc.editAuthor },
            impressions: { stringValue: doc.editImpressions },
            date: { stringValue: doc.editDate },
        }
    };

    try{
        await axios.patch(url, body);
        // 更新後、一覧をリロード
        await getData();
    } catch (err) {
        console.error('更新エラー:', err);
    }
};

onMounted(getData);

</script>

<template>
    <div class="container">
        <h1>{{ pgtitle }}</h1>
        <ul v-if="db.length">
            <li v-for="doc in db" :key="doc.name">
                <!-- 編集用フィールド -->
                <input type="text" v-model="doc.editTitle" placeholder="タイトル" />
                <input type="text" v-model="doc.editAuthor" placeholder="著者" />
                <input type="text" v-model="doc.editImpressions" placeholder="感想" />
                <input type="date" v-model="doc.editDate" placeholder="日付" />
               
                <!-- 更新ボタン -->
                <button @click="updataDoc(doc)" style="margin-left:8px">更新</button>
            </li>
        </ul>
        <p v-else>ドキュメントがありません。</p>
    </div>
</template>
