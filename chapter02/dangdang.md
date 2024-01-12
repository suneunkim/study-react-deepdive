# 02장: 리액트 핵심 요소 깊게 살펴보기

책을 읽기 전에 자바스크립트에 대한 기본적인 지식을 다루는 장입니다.
이 장에서는 리액트를 사용하면서 자주 사용하는 자바스크립트 문법과 개념을 간단히 정리합니다.

<br>

- [02장: 리액트 핵심 요소 깊게 살펴보기](#02장-리액트-핵심-요소-깊게-살펴보기)
  - [2.1 JSX란?](#21-jsx란)
    - [2.1.1 JSX의 정의](#211-jsx의-정의)
    - [2.1.2 JSX 예제](#212-jsx-예제)
    - [2.1.3 JSX는 어떻게 자바스크립트에서 변환될까?](#213-jsx는-어떻게-자바스크립트에서-변환될까)
    - [2.1.4 정리](#214-정리)
  - [2.2 가상 DOM과 리액트 파이버](#22-가상-dom과-리액트-파이버)
    - [2.2.1 DOM과 브라우저 렌더링 과정](#221-dom과-브라우저-렌더링-과정)
    - [2.2.2 가상 DOM의 탄생 배경](#222-가상-dom의-탄생-배경)
    - [2.2.3 가상 DOM을 위한 아키텍처, 리액트 파이버](#223-가상-dom을-위한-아키텍처-리액트-파이버)
    - [2.2.4 파이버와 가상 DOM](#224-파이버와-가상-dom)
    - [2.2.5 정리](#225-정리)
  - [2.3 클래스형 컴포넌트와 함수형 컴포넌트](#23-클래스형-컴포넌트와-함수형-컴포넌트)
    - [2.3.1 클래스형 컴포넌트](#231-클래스형-컴포넌트)
    - [2.3.2 함수형 컴포넌트](#232-함수형-컴포넌트)
    - [2.3.3 함수형 컴포넌트 vs. 클래스형 컴포넌트](#233-함수형-컴포넌트-vs-클래스형-컴포넌트)
    - [2.3.4 정리](#234-정리)
  - [2.4 렌더링은 어떻게 일어나는가?](#24-렌더링은-어떻게-일어나는가)
    - [2.4.1 리액트의 렌더링이란?](#241-리액트의-렌더링이란)
    - [2.4.2 리액트의 렌더링이 일어나는 이유](#242-리액트의-렌더링이-일어나는-이유)
    - [2.4.3 리액트의 렌더링 프로세스](#243-리액트의-렌더링-프로세스)
    - [2.4.4 렌더와 커밋](#244-렌더와-커밋)
    - [2.4.5 일반적인 렌더링 시나리오 살펴보기](#245-일반적인-렌더링-시나리오-살펴보기)
    - [2.4.6 정리](#246-정리)
  - [2.5 컴포넌트와 함수의 무거운 연산을 기억해 두는 메모이제이션](#25-컴포넌트와-함수의-무거운-연산을-기억해-두는-메모이제이션)
    - [2.5.1 주장 1: 섣부른 최적화는 독이다, 꼭 필요한 곳에만 메모이제이션을 추가하자](#251-주장-1-섣부른-최적화는-독이다-꼭-필요한-곳에만-메모이제이션을-추가하자)
    - [2.5.2 주장 2: 렌더링 과정의 비용은 비싸다, 모조리 메모이제이션해 버리자](#252-주장-2-렌더링-과정의-비용은-비싸다-모조리-메모이제이션해-버리자)
    - [2.5.3 결론 및 정리](#253-결론-및-정리)

<br>

## 2.1 JSX란?
> 페이스북에서 만든 새로운 구문, 트랜스파일러를 거쳐 자바스크립트 코드로 변환된다.

> XML 형식과 비슷

### 2.1.1 JSX의 정의
> JSXElement: 가장 기본 요소, HTML의 element와 비슷한 역할

> JSXAttributes: JSXElement에 부여할 수 있는 속성

> JSXChildren: JSXElement의 자식 값

> JSXString: JSX에서 정의된 문자열

### 2.1.2 JSX 예제

### 2.1.3 JSX는 어떻게 자바스크립트에서 변환될까?
> @babel/plugin-transform-react-jsx를 통해 jsx구문을 자바스크립트가 이해할 수 있는 형태로 변환

### 2.1.4 정리

<br>

## 2.2 가상 DOM과 리액트 파이버

### 2.2.1 DOM과 브라우저 렌더링 과정
> HTML을 parsing 해서 DOM 생성, CSS를 parsing 해서 CSSOM 생성

### 2.2.2 가상 DOM의 탄생 배경
> 가상 DOM은 실제 DOM의 업데이트를 효율적으로 하기 위해 등장

### 2.2.3 가상 DOM을 위한 아키텍처, 리액트 파이버
> 가상DOM과 실제 DOM을 비교해 변경 사항을 수집하여 차이가 있는 부분을 화면에 재렌더링 요청하는 역할.

> 웹 페이지가 표시할 DOM을 메모리에 만든 후 업데이트 할 시점에 실제 DOM에 반영한다.

> current와 workInProgress 두 트리를 운용하며, workInProgress에 렌더링 후 더블 버퍼링을 사용하여 화면을 업데이트한다. 

### 2.2.4 파이버와 가상 DOM
> 리액트 컴포넌트에 대한 정보를 1:1로 가지고 있다.

> 비동기로 수행



### 2.2.5 정리

<br>

## 2.3 클래스형 컴포넌트와 함수형 컴포넌트

### 2.3.1 클래스형 컴포넌트
> React.Component 또는 React.PureComponent를 상속해 생성

> 생명 주기 메서드가 존재

> 한계
>   > 데이터의 흐름을 추적하기 어렵다.
>   > 애플리케이션 내부 로직의 재사용이 어렵다.

### 2.3.2 함수형 컴포넌트
> 간결함.

### 2.3.3 함수형 컴포넌트 vs. 클래스형 컴포넌트
> 생명주기 메서드의 부재

> 함수형 컴포넌트는 렌더링이 일어날 때마다 그 순간의 props와 state를 기준으로 렌더링된다, 클래스형 컴포넌트는 시간의 흐름에 따라 변하는 this를 기준으로 렌더링이 일어난다.

### 2.3.4 정리

<br>

## 2.4 렌더링은 어떻게 일어나는가?

### 2.4.1 리액트의 렌더링이란?
> 모든 컴포넌트들이 현재 자신들의 props와 state를 기반으로 어떻게 UI를 구성하고 이를 바탕으로 어떤 DOM 결과를 브라우저에게 제공할 것인지 계산하는 일련의 과정

### 2.4.2 리액트의 렌더링이 일어나는 이유
> 최초 렌더링: 어플리케이션 진입 시

> 리렌더링: 최초 렌더링 이후 모두

### 2.4.3 리액트의 렌더링 프로세스
> 루트부터 트리를 따라 업데이트가 필요한 모든 컴포넌트를 찾는다.

> 가상 DOM과 실제 DOM과 비교해 변경사항 수집(재조정)

> 모든 변경사항을 하나의 동기 시퀀스로 DOM에 적용
### 2.4.4 렌더와 커밋

### 2.4.5 일반적인 렌더링 시나리오 살펴보기

### 2.4.6 정리

<br>

## 2.5 컴포넌트와 함수의 무거운 연산을 기억해 두는 메모이제이션

### 2.5.1 주장 1: 섣부른 최적화는 독이다, 꼭 필요한 곳에만 메모이제이션을 추가하자

### 2.5.2 주장 2: 렌더링 과정의 비용은 비싸다, 모조리 메모이제이션해 버리자

### 2.5.3 결론 및 정리