---
title: "2주프로젝트 회고"
category:
  - retrospect
tag:
  - codestates
---

# 2주 프로젝트 Project Recollect 회고

2주 프로젝트가 끝났다. 프로젝트를 시작할 때는 무엇부터 시작해야할 지 막막했는데 SR을 하고 기능을 차근차근 구현하다보니 어찌어찌 완성하게 되었다.

같이 프로젝트한 팀원분들께 고생했고, 고맙다는 말을 전하고 싶다.

이번 프로젝트는 한마디로 다사다난이다. 정말 많은 문제상황들을 겪었고 덕분에 4주 프로젝트에서 반면교사 삼을만한 교훈들을 많이 얻게되었다.

## Project Recollect

Recollect는 기존의 북마크 서비스의 불편한 점을 개선해 차별화한 북마크 아카이브 서비스다.

이 서비스의 주요한 기능은 알람을 통해 읽지 않고 잊혀져가는 북마크를 관리할 수 있게하는 것이다.

## 프로젝트 진행하면서 느낀 4가지.

### 1. SR은 정말 중요하다.

정말 꼼꼼하게 SR을 했다고 생각했는데 아니었다. 우선 우리가 구현할 기능에 대해 API 작성을 빠뜨린것도 몇 개 있었다.

소셜로그인 과정에서 필요한 엑세스토큰을 요청하는 API, 리프레쉬 토큰을 전달하는 API, 북마크의 조회수를 올려주는 API 등등.

물론 첫 프로젝트이고 서비스를 구현하는 와중에 이것들이 필요하다는 것을 깨달았기 때문에 기획단계에서 이 API를 만드는 것을 할 수는 없었다.

이제 4주프로젝트에서는 정말 빈틈없이 기획을 해야겠다 다짐한다.

목업을 만들지 않은게 실수다. 처음부터 프레임을 짜고 그 다음 분업해서 작업을 해야했다. miro에서 그림으로 프레임을 만들었지만 그것만으로는 충분하지 않았다.

프로젝트를 진행하면서 4주 final project에서는 직접 html태그로 목업을 짜고 css로 위치를 정하고 시작해야겠다고 팀원들과 내내 이야기 했다.

목업을 맞추지 않고 바로 코드를 작성한 결과 마지막까지 여기 버튼 크기 이상한데? 박스 위치가 여기가 맞을까? 마지막까지 고민하게 되었다.

### 2. 회의를 길게한다고 소통이 잘되는 건 아니더라.

회의시간은 짧으면서 효율적인게 최고다.

이번 팀프로젝트는 디스코드를 통해 화상채팅, 화면공유을 많이 했다. 즉각즉각 소통이 된다는 점과 팀워크에 도움이 되는 점은 아주 좋았다.

그러나 회의 시간을 너무 길게 가지게 되니 나중에는 지쳐 집중도가 떨어지고 그 영향으로 팀원 서로 다르게 이해하는 부분이 하나하나 생기게 되었다.

서버에서 보내주는 정보를 잘못 이해해서 클라이언트에서 아예 다르게 정보에 접근했고 거의 마지막에 부랴부랴 코드를 고치게 되었다.

생각해보면 거의 대부분 서버와 클라이언트 연결이 잘 되는지 확인하려고 회의를 진행했는데 다시 생각해보면 굳이 모여서 확인하지 않고 각자 서버의 코드를 pull해와서 확인해도 됐었다.

### 3. 클라이언트에서 서버를, 서버에서 클라이언트를 테스트하며 코드를 짜야한다.

이건 너무 당연한 소리지만 놀랍게도 클라이언트에서는 서버와의 연결없이 테스트를 진행했다. (왜 그렇게 했는지, 중간에 방법을 개선해볼 여지는 없었는지 반성하게 된다.)

이는 정말 큰 비효율을 낳았다. 회의를 길게한 것도 바로 이 때문이다. 서버와의 연결을 확인해보지 않았기 때문에 에러를 잡느라 회의가 길어졌다.

각자 서버와 연결을 확인해보며 작업을 했더라면. 미처 파악하지 못한 에러핸들링을 마주칠 필요가 없었다. 그래서 우리 팀이 진행했던 회의 중 절반 이상은 사실 쓸모가 없다. 이제라도 그 사실을 깨달았으니 4주 프로젝트에서는 더 효율적으로 진행할 수있을 거라 기대한다.

### 4. 리액트 상태관리는 정말 어렵다.

상태관리가 철저하게 꼬였다. 부모컴포넌트에 있는 메서드를 저 밑까지 끌어내리다 보니 정말 헷갈렸고, 간혹 메서드끼리 서로를 호출하는 코드를 작성해 무한루프에 빠지기도 했다. 클래스 컴포넌트를 사용하다보니 메서드를 사용할 때마다 this를 bind해줘야하는 부분도 매우 성가셨다. 4주프로젝트에서는 리덕스를 공부해서 사용하자고 팀원들끼리 다짐했다.

## 그래도 잘했다.

위에서 너무 쓴소리만 적은 것 같으니 마지막은 훈훈하게 마무리 해야겠다. 열정있는 팀원들, 서로를 배려하고 으쌰으쌰하는 분위기 덕분에 힘들지만 재밌게 프로젝트를 진행한거 같다. 4주 프로젝트에서 개선할 점은 개선하고, 이 분위기를 이어가면 성공적으로 마무리할 것이라 믿는다.
