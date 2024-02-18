<template>
  <h1 class="title">Q&A with <span class="red">B</span><span class="yellow">E</span><span class="blue">R</span><span class="green">T</span></h1>  
  <div class="loading-container" v-if="model_loading">
    <Loading />
    <h1 v-if="!model_successflly_loaded">Failed to load model</h1>
  </div>
  <table border="1">
    <tr>
      <th class="top-left">
        <div class="passage">Passage</div>
      </th>
      <th class="top-right">
        <div class="question">Question</div>
      </th>
    </tr>
    <tr>
      <td class="bottom-left">
        <textarea v-model="passage" rows="10" cols="50"></textarea>
      </td>
      <td class="bottom-right">
        <textarea v-model="question" rows="10" cols="50"></textarea>
      </td>
    </tr>
  </table>
  <div class="result">
    <h1>Result: {{ answer }}</h1>
  </div>
</template>

<style>
body {
  background-color: #FFFFFF;
}

.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed; /* 画面に対して固定位置 */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.9); /* 背景をわずかに透明にして、他の要素を覆う */
  z-index: 1000; /* 他の要素よりも前面に表示 */
}

Loading {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.red {
  color: #B11322;
}
.yellow {
  color: #F4A41A;
}
.blue {
  color: #2368B3;
}
.green {
  color: #118335;
}

table {
  height: 480px;
  width: 100%;
  border-collapse: separate;
  overflow: hidden;
  margin-top: 10px;
  border-radius: 24px;
  border-spacing: 0;
}

.top-left {
  border-top: none;
  border-bottom: none;
  border-left: none;
  border-right: none;
}

.top-right {
  border-top: none;
  border-bottom: none;
  border-left: none;
  border-right: none;
}

.bottom-left {
  border-bottom: none;
  border-left: none;
  border-right: none;
}

.bottom-right {
  border-bottom: none;
  border-right: none;
}

textarea {
  width: 100%;
  height: 100%;
  padding: 12px 20px;
  margin-top: 8px;
  box-sizing: border-box;
  border: none;
  resize: none;
}

</style>

<script lang="ts" setup>
import * as tf from '@tensorflow/tfjs';
import * as qna from '@tensorflow-models/qna';

const answer = ref('');
const model_loading = ref(true);
const model_successflly_loaded = ref(true);
let model = null;
const question = ref('');
const passage = ref('');

onMounted(async () => {
  tf.setBackend("webgl")
  try {
    model = await qna.load();
    model_loading.value = false;
  } catch (e) {
    console.error(e);
    model_successflly_loaded.value = false;
    model_loading.value = true;
  }
  question.value = 'What is the capital of France?';
  passage.value = 'The capital of France is Paris. I live in Paris. And the capital of Spain is Madrid. My friend is from Madrid.';
});

watch([question, passage], async () => {
  if (!model_loading.value) { // モデルがロードされていることを確認
    const answers = await model.findAnswers(question.value, passage.value);
    answer.value = answers.length > 0 ? answers[0].text : 'No answer found.';
  }
});
</script>