
import type { Loading } from '#build/components';
<template>
    <div class="loader">
          <div></div>
          <div></div>
          <div></div>
        </div>
  </template>
  
  <script>
  export default {
    // コンポーネントの設定
  };
  </script>
  
  <style scoped>
@keyframes loading {
  0% {
    -webkit-transform: scale(1);
            transform: scale(1); }
  50% {
    -webkit-transform: scale(0.5);
            transform: scale(0.5);
    opacity: 0.7; }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 1; } }

.ball-grid-pulse {
  width: 100px; }
.loader > div:nth-child(1) {
            animation-delay: 0s;
            animation-duration: 1s; }
.loader > div:nth-child(2) {
            animation-delay: 0.20s;
            animation-duration: 1s; }
.loader > div:nth-child(3) {
            animation-delay: 0.40s;
            animation-duration: 1s; }
.loader > div {
    background-color: black;
    width: 30px;
    height: 30px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    display: inline-block;
    float: left;
    -webkit-animation-name: loading;
            animation-name: loading;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite;
    -webkit-animation-delay: 0;
            animation-delay: 0; }
</style>
  