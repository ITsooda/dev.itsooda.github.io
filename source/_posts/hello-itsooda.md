---
title: Hello World
author: JunMon
---
### 개요

Hexo : Jekyll 과 같은 블로그 generator

우리 블로그를 호스팅하는 실제 repository : https://ITsooda/itsooda.github.io

우리가 블로그를 개발할 repository : https://ITsooda/dev.itsooda.github.io

[Hexo Docs](https://hexo.io/ko/docs/)



### hexo 동작원리 (node/npm을 전제)

1. npm 을 통해 hexo를 글로벌 설치한다.

   ```shell
   npm install hexo-cli -g
   ```

2. 로컬에 hexo 프로젝트를 생성한다. (이걸 한게 https://ITsooda/dev.itsooda.github.io)

3. 해당 디렉토리에서 npm install 을 해준다. (노드 모듈 설치)

4. post를 작성한다

   - 현재는 souce/_post 디렉토리 내에 md 로 저장
   - cli에서 'hexo new post 제목' 과 같은 식으로 포스트를 만들 수 있다.

5. npm 을 통해 hexo-deployer-git을 해당 경로에  설치해준다

   ```shell
   npm install hexo-deployer-git --save
   ```

6. 아래와 같이 generate(일종의 컴파일이라고 생각하면 쉬움) => deploy (github레포에 push, submit이라고 생각하자) 한다.

   ```shell
   hexo generate
   hexo deploy
   ## hexo g -d 혹은 hexo deploy --generate 와 같이 쓸수도 있다.
   ```

7. 자! 그럼 이제 해당 디렉토리에 .deploy_git 이란 경로가 만들어지고 itsooda.github.io repository엔 해당 경로의 파일들이 업로드 된다.
   이제 https://itsooda.github.io 로 접속해보자!

8. 그런데 우리는 **팀블로그**를 운영해야한다. 우리 로컬에 있는 hexo 프로젝트를 다른 사람들에게도 전달해야겠쥬? 그렇다면 이제 hexo 프로젝트의 루트 디렉토리에서 [dev repository](https://ITsooda/dev.itsooda.github.io)로 add => commit => push !



### To Do

- 지금은 dev 레포안에 있는 모든 asset 과 컨텐츠를 공유하지만 (물론 최소한의 .gitignore를 했음) 차후에는 post 만 git 을 통해 함께 push 해야할듯
- 테마가 너무 많다! 그런데 참 아쉽게도 hexo가 중국/대만권에서 워낙 대세라서 중국어 자료가 많다... 아무튼! 다들 원하는 테마가 있다면 말해줬으면 좋겠습니다~!
  [hexo 테마 사이트](https://hexo.io/themes/)