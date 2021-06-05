---
title : "[Review] Attention Is All You Need"
excerpt: "Beginning of the Era of Attention Module"
toc: true
header:
  overlay_image : /assets/images/overlay_image.jpg
---
# Intuition
- 언어 모델링과 기계번역 분야에서 RNN-based model들이 우위를 차지하고 있었으나, 모델 특성상 병렬처리가 불가능하다는 단점이 존재함  <br>
- 이전 모델들중 attention mechanism을 부분적으로 사용한 모델이 존재하였음 <br>
&#8594; 따라서, 저자들은 attention module만을 사용하여 병렬처리가 가능하도록하여 성능향상과 학습시간 단축을 이루는 모델을 재시함   

# Model Architecture
- 대부분의 언어 모델링 혹은 기계번역 모델들이 채택하는 encoder-decoder 구조를 가지고 있음
<p align="center">
  <img src="/assets/images/posts/transformer/model_architecture.png">
</p>

## Encoder and Decoder
- Encoder과 decoder모두 N개의 층을 쌓아 사용하고 있으나, decoder에서는 sublayer로 masked multi-head attention을 추가적으로 사용한다는 점에서 차이가 있음
- 따라서, encoder에서는 multi-head attention과 feed forward만을 내부층으로 사용하고, decoder에서는 추가로 masked multi-head attention을 각 층의 첫 부분에 추가함
- 각 내부층에서 계산이 끝난 후에는 skip connection결과를 더하여 layer normalization을 적용함

# Attention Module
$$\text{Attention(}Q, K, V\text{) }= \text{softmax(}\frac{QK^T}{\sqrt{d_k}})V   $$
<p align="center">
  <img src="/assets/images/posts/transformer/attention.png">
</p>
- 우선 attention이란, query와 (key, value)를 입력으로 받아, query와 key의 관련성을 기반으로 하는 weight값들을 value에 곱하여 출력하는 함수임
- attention으로는 dot-product attention과 additive attention이 있으며, 이론적으로는 비슷한 time complexity를 가지고 있지만, optimized matrix operations를 기반으로 dot-product attention이 훨씬 효율적임
- 하지만 dot-product attention은 gradient vanishing문제에 취약하였고, 이 논문에서는 위의 식처럼 weight을 곱해주어 성능, 속도, 효율성을 한번에 해결함

## Multi-head Attention
$$\text{head}_i = \text{Attention(}QW^Q_i, KW^K_i, VW^V_i), i = 1, \cdots, h$$   
$$\text{MultiHead(}Q,K,V\text{) = Concat(head}_1,\cdots,\text{head}_h)W^O  $$
- Q, K, V를 직접 사용하기보단, 성능 향상을 목표로 h개의 attention들의 결과를 concatenate한 후, weight $$W^O$$를 사용하여 attention module의 input과 multi-head attention의 output이 동일한 shape을 가지도록함

# Positional Encoding
$$\text{PE}_{(pos,2i)}=sin(pos/10000^{2i/d_{model}}\text{)}$$
- RNN 혹은 CNN 계열 구조를 제거했기때문에, position을 기억하는 수단이 사라짐
- 위와같은 사인함수를 이용하여, embedding 된 input값에 position을 따로 입력해주어 위의 문제를 해결함






<!-- to change the font size, look at _page.scss, then search .page__content, line 110 - 112 --> 
