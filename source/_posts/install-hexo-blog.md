---
title: Hexo 블로그 설치 및 Hueman 테마 적용
thumbnail: /thumbnails/hexo.png
tag: Hexo
# toc: true
categories:
- [Hexo]
---

# Hexo Blog 설치

### 1. Github에 블로그용 Repo를 생성한다.

![](/images/create-repo.PNG)


### 2. [Hexo 공식 문서](https://hexo.io/docs/)의  Installation 부분을 따라 Hexo를 설치한다 ([Node.js](https://nodejs.org/en/), [Git](https://git-scm.com/) 설치 필요).

```bash
# Install Hexo
$ npm install -g hexo-cli

# Advanced Installation and usage
$ npm install hexo
```

(저는 가볍게 `hexo-cli`만 설치하겠습니다.)

### 3. 블로그 프로젝트를 위한 로컬 디렉토리를 생성한다.

```bash
# 원하는 경로로 이동한 후 블로그 디렉토리 생성
$ cd ~
$ mkdir smilejin92
```

### 4. 3에서 생성한 디렉토리로 이동하여 Hexo Blog를 [초기화](https://hexo.io/docs/commands)한다.

```bash
$ cd smilejin92
$ hexo init
```

### 5. 4에서 생성된 `packge.json` 파일에 명시되어 있는 dependencies 모듈을 설치한다.

```bash
$ npm install
```



### 6. 블로그가 잘 설치되었는지 [로컬 서버를 구동](https://hexo.io/docs/commands)시켜 확인한다 ([localhost:4000](localhost:4000)).

```bash
$ hexo server
```

![](/images/init-blog.PNG)



위와 같은 화면이 나온다면 서버가 정상적으로 동작하는 것이다.



### 7. 서버를 종료시키고 `_config.yml` 파일을 열어 본인의 정보에 맞게 내용물을 수정한다.

![](/images/config.PNG)



### 8. 7에서 수정한 내용을 저장한 후 서버를 다시 구동한다.

![](/images/config-blog.PNG)

메인 배너에 수정한 내용이 반영되어 있는 것을 확인할 수 있다.



# Hueman Theme 적용

Hexo Blog 설치시 기본으로 제공되는 테마는 landscape이다.

![](/images/directory.PNG)



[Hexo 사이트](https://hexo.io/themes/)에서 마음에 드는 테마를 찾아보자 (본 예제는 [Hueman](https://blog.zhangruipeng.me/hexo-theme-hueman/)테마를 사용).

![](/images/themes.PNG)



[Github의 Hueman 테마 Repo](https://github.com/ppoffice/hexo-theme-hueman)에서 Hueman 테마가 제공하는 기능들을 확인할 수 있다.

![](/images/preview.PNG)



### 1. theme 폴더로 이동하여 Hueman 테마를 클론한다.

```bash
$ cd themes
$ git clone https://github.com/ppoffice/hexo-theme-hueman.git
```



### 2. 클론이 완료되면 themes 폴더 안에 hexo-theme-hueman 테마가 생성된다.

![](/images/new-directory.PNG)



### 3. 새롭게 생성된 테마 폴더로 이동하여 테마의 packge.json 파일에 명시되어 있는 dependencies 모듈을 설치한다.

```bash
$ cd hexo-theme-hueman
$ npm install
```

또한 Hueman 테마에서 제공하는 Insight Search 기능을 사용하기 위해서는 아래와 같이 모듈을 하나 더 설치해야한다.

```bash
$ npm install -S hexo-generator-json-content
```



### 4. 설치가 완료되면 최상위 폴더로 이동하여 _config.yml 파일의 theme 부분을 hexo-theme-hueman으로 수정한다.

![](/images/config-theme.PNG)



### 5. hexo-theme-hueman 폴더 안의 `_config.yml.example` 파일명을 `_config.yml`로 변경한다 (4의 `_config.yml`과 다른 파일임에 유의).

![](/images/config-example.PNG)



### 6. Hexo 서버를 구동시켜보자.

```bash
$ hexo server
```

![](/images/hueman.PNG)

위와 같은 화면이 나온다면 hueman 테마가 정상적으로 적용된 것이다.



다음 포스트에서는 설치한 Hueman 테마를 커스터마이즈 해보는 방법에 대해 알아볼 것이다.