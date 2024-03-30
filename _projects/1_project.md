---
layout: distill
title: OUTLOST
description: 1인칭 시점의 생존 호러 게임 아웃라스트를 레퍼런스 삼아 진행한 첫 번째 게임 프로젝트
img: assets/img/sesac_project01/thumbnail.gif
importance: 5
category: SESAC(2308 ~ 2403)
# related_publications: true
# pretty_table: true
toc:
  - name: 프로젝트 영상
  - name: 프로젝트 기록(Notion)
  - name: 개요
  - name: 진행 내용
  - name: 결과
images:
  compare: true
  slider: true
---

## 프로젝트 영상

[![프로젝트 영상 보기](https://img.youtube.com/vi/5A8AQsEz_eE/0.jpg)](https://youtu.be/5A8AQsEz_eE "프로젝트 영상 - 클릭하여 시청")

---

## 프로젝트 기록(Notion)

<a href="https://www.notion.so/1-Outlost-4c242463d4734705afd4a9965d3a31ef" target="_blank">프로젝트 Notion 페이지 바로가기</a>

---

## 개요

|       Duration       |      Team Size     |      SKILL      |
| :------------------: | :----------------: | :-------------: |
| 23/10/04 ~ 23/11/06  |         2          |    UE5.3, BP    |

1인칭 시점의 생존 호러 게임 아웃라스트를 모작하여 진행한 첫 번째 게임 프로젝트.
플레이어는 밀폐된 공간 안에서 자신을 공격하는 ai를 피해 도망치며 숨겨진 퍼즐을 풀고 탈출하는 내용의 게임.

---

## 진행 내용

### 모작할 게임 선택

새싹 XR 과정에서 언리얼엔진 교육을 시작하며 첫 번째 프로젝트로 모작을 진행하였다. 블루프린트 조차 익숙하지 않은 상황이었기 때문에 전투가 없고, 시스템이 복잡하지 않은 게임을 위주로 후보를 추렸다. 그 중에 플레이 영상도 많고, 참고할만한 레퍼런스가 많았던 '아웃라스트'를 선택했다.

### 역할 분담

우리팀은 2명으로, 크게 플레이어와 ai로 나누어 역할을 분담하였고, 맵과 아이템 등은 플레이어 기능을 담당한 내가 작업을 진행했다.

### 작업 진행

작업 기한이 34일이었는데, 기획발표를 하고 거의 10일 정도 아무것도 못하고 스터디를 진행했다. 그리도 10일 정도는 캐릭터 리타겟팅과 이동, 달리기, 앉기 등 기본 동작을 구현했다.

### Function override

우리 게임은 플레이어가 상호작용할 수 있는 객체가 아이템, 문, 숨는 장소로 크게 3가지로 구분되었다. 하지만 당시에는 C++에 대한 지식이나 OOP에 대한 지식이 전혀 없었기 때문에 상호작용을 구현하기 위해 며칠을 고민했다.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project01/img1.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

고심 끝에 부모 클래스를 사용하여 조준선이 객체에 포커싱 되었을 때, 함수 오버라이드로 Hint massage를 제공하고, 마우스 좌클릭 하였을 때 행동하도록 설계했다.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project01/img2.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project01/img3.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

### Night vision

아웃라스트의 가장 큰 특징인 나이트 비전(야간 투시)을 구현하였다. 플레이어 카메라의 포스트 프로세스 메테리얼을 추가하여, 모드를 토글식으로 작동 시켰다.

<img-comparison-slider>
  {% include figure.liquid path="assets/img/sesac_project01/img4.jpg" class="img-fluid rounded z-depth-1" slot="first" %}
  {% include figure.liquid path="assets/img/sesac_project01/img5.jpg" class="img-fluid rounded z-depth-1" slot="second" %}
</img-comparison-slider>

왼쪽이 원작, 오른쪽이 모작이다. 머터리얼의 구조는 웹 문서를 참고했다.

---

## 결과

마지막 10일 정도 버그 리포트와 수정하는 방향으로 스케줄을 진행했다. 수정 사항이 30개 정도 나왔는데, 80% 정도 수정하면서 나름 성공적으로 프로젝트를 마무리할 수 있었고, 학원에서 진행한 인기투표에서도 1위를 하는 좋은 결과를 만들 수 있었다.
