---
title: Introduction
author: sam
date: 2022-12-16 16:32:00 -0500
categories: [Library, React Leaflet]
tags: [React Leaflet, Leaflet]
imgp: /assets/img/git/
---

# Introduction

## Core concepts

React Leaflet은 Leaflet을 React에 바인딩하는 라이브러리로, Leaflet의 대체제가 아닌 Leaflet의 레이어를 React 컴포넌트로 추출하는데 사용된다. 다른 리액트 컴퍼넌트와 다르게 동작할 수 있음을 유의하자.

### DOM rendering

**React는 Leaflet 레이어를 DOM에 렌더하지 않으며, 이 렌더링은 Leaflet 자체에서 수행**된다. React는 `MapContainer` 컴포넌트와 UI 레이어 컴포넌트를 렌더링할 때 단지 div 요소만을 렌더한다.

### **Component properties**

컴포넌트에 넘겨진 props는 컴포넌트가 최초 렌더될 때 연결된 Leaflet 인스턴스를 생성하는데 사용되며 기본적으로 불변으로 처리돼야 한다.

최초 렌더 중에 모든 props는 Leaflet에 의해 있는 그대로 사용할 수 있게 지원되지만, props가 변경가능한 것(being mutable)이란 명시적인 문서화를 않는이상 UI에서 props는 업데이트되지 않는다.

변경 가능한 prop의 변경사항은 (별도로 명시하지 않는 이상) 참조로 비교되며, Leaflet 요소 인스턴스의 관련 메서드를 호출하여 적용된다.

### **Leaflet elements references**

별도로 명시하지 않는한 React Leaflet으로 export된 모든 컴포넌트는 DOM 요소 또는 생성된 Leaflet 요소 인스턴스를 노출하는 `ref` 를 지원한다.

이를 통해 애플리케이션은 필요할 때 Leaflet의 명령형 API에 액세스할 수 있지만, 설정한 props와 불일치가 발생할 수 있으므로 신중하게 사용해야 한다.

### **React context**

React Leaflet은 React의 context API를 사용하여 Leaflet 요소 인스턴스를 자식 요소가 사용할 수 있도록 한다.

각 Leaflet 맵 인스턴스는 `MapContainer` 컴포넌트에 의해 생성된 각자의 React context를 갖는다. React Leaflet에서 제공하는 다른 컴포넌트와 hook은 `MapContainer` 의 하위 컴포넌트만 사용할 수 있다.

## **Lifecycle process**

1. `MapContainer` 는 맵의 컨테이너 요소인 `<div>` 를 렌더링한다. `placeholder` prop이 설정되면 `<div>` 컨테이너 내에 렌더링된다.
2. `MapContainer` 컴포넌트 props와 함께 생성된 `<div>` 에 대한 Leaflet 맵을 인스턴스하고 해당 맵 인스턴스를 포함하는 React context를 생성한다.
3. `MapContainer` 가 자신의 자식 요소들을 렌더링한다.
4. 각 자식 컴포넌트는 자신의 context와 props를 사용해 요소에 대한 매칭된 Leaflet 인스턴스를 인스턴스하고 이를 맵에 추가한다.
5. 자식 컴포넌트가 리렌더 될 때, 지원되는 변경가능 props에 대한 변경사항은 맵에 적용된다.
6. 컴포넌트가 렌더 트리에서 제거될 때, 필요에 따라 맵에서 자신의 레이어를 제거한다.

## **Limitations**

- Leaflet은 로드 될 때 DOM을 직접 호출하는 방식으로 작동한다. 따라서 서버 사이드 렌더링과 호환되지 않는다.
- 드러난 컴포넌트는 DOM 요소가 아닌 Leaflet 레이어에 대한 추상화다. 이 중 몇몇은 Leaflet이 제공하는 setter을 호출해 곧바로 업데이트할 수 있는 prop을 갖는 반면, 다른 컴포넌트는 리액트 알고리즘에 따라 고유한 값을 `key` prop에 할당