---
layout: post
title: a post with code diff-copy
date: 2024-01-27 19:22:00
description: this is how you can display code diffs
tags: formatting code
categories: sample-posts
code_diff: true
---

https://github.com/topics/jekyll-theme 에서 다양한 테마를 구경할 수 있다. 테마 마다 설치 방법이 조금씩 다를 수 있으니, readme 파일을 꼭 읽어보길 바람!

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/1.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    내가 선택한 al-folio 템플릿. 깔끔한 쿠앤크에 반응형 디자인이라 마음에 들었다.
</div>

테마를 선택하면 템플릿 레포지토리로 이동할 수 있다. 오른쪽 상단의 `Use this template` 드롭다운 버튼을 클릭하면 `Create a new repository`을 선택할 수 있다.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/2.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

다른 항목은 크게 건들 필요가 없고, Repository name만 입력해주면 되는데, `<your-github-username>.github.io` 형식에 맞춰 작성해주면 된다. 필자는 `mhoo999.github.io`로 작성했다.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/3.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

레포지토리가 생성됐다면 `Settings > Actions > General`로 이동하자. 아래로 내리면 Workflow permissions 컨테이너를 확인할 수 있는데, `Read and write permissions` 버튼을 클릭하여 워크플로우가 모든 범위에 대해 읽기 및 쓰기 권한을 가지도록 설정하고 저장해주자.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/4.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/5.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

다음은 코드로 이동하여 `_config.yml`을 찾아보자. 19번째 줄에서 `url:`을 찾을 수 있다. 우리가 사용할 주소로 입력해주면 된다. 아래 `baseurl:`은 비워두자.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/6.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

다시 `Settings`로 돌아가서 LMB의 `Pages`항목을 클릭한다. Build and deployment 컨테이너에서 `Source`가 `Deply from a branch`인지 확인하고, 아래 브랜치를 `gh-pages`로 변경하고 저장해주자.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/7.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

이제 마무리다. 모든 설정이 완료되면 주소창에 url을 입력하고 엔터를 눌러보자. 다음과 같이 데모페이지를 확인할 수 있을 것이다.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/240312/8.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

다음 과정은 로컬에 클론을 만들고, 내 입맛대로 블로그를 꾸며주면 된다.