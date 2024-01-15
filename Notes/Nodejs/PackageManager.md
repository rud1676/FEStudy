# 패키지 매니저 비교

> author: @OOWGNOD

- [0. node_modules]
  - [0-1. 블랙홀보다 더 깊은 node_modules]
- [1. npm이란?]
  - [1-1. npm의 주요 명령어]
  - [1-2. npx]
  - [1-3. npm의 문제점 및 개선된점]
  - [1-4. 이외 문제에 대한 사건]
- [2. yarn이란?]
  - [2-1. yarn의 주요 명령어]
  - [2-2. facebook이 말하는 yarn]
- [3. npm과 yarn]
  - [3-1. 공통 명령어]
  - [3-2. 두 패키지 매니저간의 차이]
- [4. yarn berry?]
  - [4-1. Plug'n'Play]
  - [4-2. ZipFS]
  - [4-3. Zero Install]
  - [4-4. 기업들이 yarn berry를 선택하는 이유]

---

- [완성본-노션링크](https://devdongwoo.notion.site/node-packge-yarn-yarn-berry-e789e417cbfa40aeb81ff9afa925f144?pvs=4)

## 0. node_modules

### 1-1. 블랙홀보다 더 깊은 node_modules

![image](https://github.com/rud1676/FEStudy/assets/86402288/0dc254c1-a7ec-4ba5-b41c-5adc99627c24)

## 1. npm(Node.js package manager)이란?

(npm의 관한 설명)

### 1-1. npm의 주요 명령어

(명령어 표, 해당 명령어 설명)
npm의 주요 명령어

| npm                             |
| ------------------------------- |
| npm install                     |
| npm install [package]           |
| npm install —save-dev [package] |

...

### 1-2. npx

(npm ? npx? 무엇을 사용해야 할까?)

### 1-3. npm의 과거 문제점 및 개선된 점

일관적이지 않은 패키지 버전
고정되지 않은 설치 순서
순차적인 설치로 인한 긴 소요시간

### 1-4. 이외 문제에 대한 사건들

이외 npm에 문제점이 밝혀진 사건2개 소개

## 2. yarn이란?

(yarn이 탄생한 이유 및 설명)

### 2-1. yarn의 주요 명령어

(명령어 표, 해당 명령어 설명)

| yarn                  |
| --------------------- |
| yarn & yarn install   |
| yarn add [package]    |
| yarn remove [package] |

...

### 2-2. facebook이 말하는 yarn

    Ultra Fast(고속)
    Mega Secure(보안)
    Super Reliable(신뢰성)
    Offline Mode(오프라인모드)
    Deterministic(결정적)
    Network Performance(네트워크 성능)
    Same Packages(동일 패키지)
    Network Resilience(네트워크 복구)
    Flat Mode(플랫 모드)

## 3. npm과 yarn

### 3-1. 공통 명령어

(공통 명령어 표, 해당 명령어 설명)

| npm      | yarn      |
| -------- | --------- |
| npm init | yarn init |
| npm run  | yarn run  |
| npm test | yarn test |

...

### 3-2. 공통 속성

package.json의 속성 설명

    name
    version
    private ...

## 4. yarn berry?

### 4-1. Plug'n'Play

유령 의존성을 해결한 PnP

### 4-2. ZipFS (Zip Filesystem)

Archive와 checksum에 관한 설명

### 4-3. Zero Install

Zero-install 기능을 적극적으로 레포지토리에 도입함으로써 변화되는 것들

### 4-4. 기업들이 yarn berry를 선택하는 이유

요즘 대세?! Monorepo와 Micro frontend architecture
