---
title: "[React] axios 심화 - instance와 interceptor"
categories: [React]
tag: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. axios interceptor 개요

## 1.1 axios interceptor 개념

<br>

## 1.2 axios interceptor 필요성

만약 호출하는 서버가 변경되었다고 가정해보자.

```js
axios.get("http://localhost:3001/todos");
axios.post("http://localhost:3001/todos", todo);
axios.delete(`http://localhost:3001/todos/${todoId}`);
```
