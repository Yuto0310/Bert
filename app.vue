<template>
  <h1 class="title">Q&A with <span class="red">B</span><span class="yellow">E</span><span class="blue">R</span><span class="green">T</span></h1>  
  <div class="loading-container" v-if="model_loading">
    <Loading />
    <h1 v-if="!model_successflly_loaded">Failed to load model</h1>
  </div>
    <div class="data">
      <div class="passage">
        <h1>Passage</h1>
        <textarea class="passage" type="text" placeholder="参考にする文章を入れてください" v-model="passage" />
      </div>
      <div class="question">
        <h1>Question</h1>
        <textarea class="question"  type="text" placeholder="質問を入れてください" v-model="question" />
      </div>
    </div>
    <div class="result">
      <h1>Result: {{ answer }}</h1>
    </div>
</template>

<style>
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
.title {
  background-color: #FFFFFF;
  margin: 0;
  text-align: center;
  font-size: 48px;
}

body {
  background-color: #FFFFFF;
}

.data {
  display: flex;
  align-items: center;
  margin: 20px;
  left: auto;
  right: auto;
}

.data div {
  margin: 20px;
}
.question, .passage {
  width: 100%;
}

h1 {
  text-align: center;
}

textarea {

  width: 80%;
  height: 300px;
  margin: 10px;
  padding: 10px;
  resize: none;
  border-radius: 10px;
}
result {
  width: 80%;
  height: 300px;
  margin: 10px;
  padding: 10px;
  resize: none;
  border-radius: 10px;
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