---
title: "Retrospective - 세미 프로젝트 회고"

date: 2021-11-13
last_modified_at: 2021-11-13


categories:
  - Java Web Programming

tags:
  - [kosta, "세미 프로젝트", project]

---

목차

* [돌아보기에 앞서](#돌아보기에-앞서)
* [첫 세미 프로젝트](#첫-세미-프로젝트)
  * [좋았던 점](#)
  * [아쉬운 점](#)
  * [스스로에 대한 평가](#스스로에 대한 평가)

* [앞으로](#앞으로)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> ## 돌아보기에 앞서

첫 세미 프로젝트의 발표를 마쳤습니다.<br>

후련하면서도 시원섭섭한 마음인데 프로젝트를 마치고 이렇게 글을 남기는 이유는 앞으로의 성장을 위해서입니다.<br>

좋았던 부분과 아쉬웠던 부분, 그리고 스스로를 가능한 객관적으로 돌아봄으로써 다음 프로젝트에는 더 좋은 모습을 갖추고자 합니다.<br>

<br>

---

> ## 첫 세미 프로젝트

세미 프로젝트는 10월 29일부터 11월 12일까지 약 2주의 시간을 갖고 6명의 인원으로 진행했습니다.<br>

그동안 배워온 내용들을 중심으로 설계부터 구현까지 전 과정을 짧은 시간 동안 진행했는데, 저희 팀은 sns 커뮤니티를 모방해서 구현해보는 것을 컨셉으로 잡았습니다.<br>

<br>

> ### 좋았던 점

이번 프로젝트를 진행하며 잘했거나 좋았던 부분을 생각하면 가장 먼저 떠오르는 것은 역시 함께한 동료들인 것 같습니다.<br>

어떤 일을 하더라도 사람 관계가 참 어려운 부분인데 여섯 명 모두가 한 마음으로 함께 목적지를 향해 나아가고 있다는 감정을 느꼈고, 프로젝트 기간동안 제게 정말 큰 힘이 되었습니다.<br>

프로젝트를 진행하기에 환경적인 여건(_장소 선정, 예상보다 길게 진행된 수업 진도 등_)이 좋지 않았음에도 팀원들의 의견이 잘 맞고, 모두 긍정적이고 열정적으로 프로젝트에 임해주었기에 마무리를 잘 지을 수 있었던 것 같습니다.<br>

<br>

특히 인상적이었던 부분은 프로젝트 대부분의 과정을 함께 진행했다는 점 입니다. <br>일반적으로 팀 프로젝트를 진행한다고 하면 시작부터 각 파트를 나눠 분업하겠지만, 이번 세미 프로젝트는 파이널 프로젝트를 대비한 연습이기도 하고 저희는 기존에 상정한 복습 기반의 실력 성장이라는 목표에 맞게 설계부터 구현(_후반부 일부 구현 제외_)까지 대부분의 과정을 함께 진행했습니다.<br>물론 모두가 같은 작업을 하려니 개발 이전의 설계 과정에서 소모되는 시간이 상당하기도 했고, 구현에서도 막히는 부분을 함께 고민하다 보면 일정이 촉박했던 것은 사실이지만, 이러한 방식으로 진행했기 때문에 결과적으로 모든 팀원들이 프로젝트에 대한 이해도가 높아질 수 있었고, 더욱 끈끈한 연대감을 가질 수 있게 된 것 같습니다.<br>

<br>

<br>

> ### 아쉬운 점

전반적으로 높은 만족도를 갖고 있기는 하지만, 프로젝트 관리 측면에서 부족했던 부분들이 아쉬움으로 남습니다.<br>

다양한 경험을 위해 trello, notion, slack, source tree 등의 툴을 사용해보고자 했는데, source tree를 제외한 다른 툴의 활용이나 간트 차트를 활용해서 관리하고자 했던 시간 관리에 소홀함이 있었고, 그러다 보니 구현 도중 코드 리뷰를 꼼꼼히 할 여유가 없었습니다. 트러블 슈팅 역시 관리를 철저히 하지 못하여 아쉬움이 남습니다.<br>

기능적인 부분들에 대한 아쉬움을 정리해보자면 아래와 같습니다.

- 요구분석 및 설계 단계(데이터 추출 및 정규화, File List 작성, 공통된 기능에 대한 고려 등)에서 보다 구체적이어야 함
- 중복되는 기능에 대한 처리
- 코딩 컨벤션을 맞추려 했으나 코드 정리가 미흡
- UI 설계와 템플릿 사용에 많은 시간을 소비했지만, 결과적으로 템플릿 사용을 포기했고 프론트 처리에 아쉬움
- 보안에 취약(DB에 저장되는 비밀번호 등 - 추후 스프링 시큐리티, 첨부 파일 공격 취약 - 확장자 제한 등의 처리 필요)

<br>

2주라는 기간이 상당히 짧게 느껴졌던 것 같습니다.  

아쉬웠던 부분들이 참 많은데, 여기에 대해서는 추가적으로 팀원들과 조율을 해서 이번 발표로 프로젝트를 마치지 않고 개선을 더 해보고자 합니다.  

<br>

> ### 스스로를 돌아보며

프로젝트 기간 동안 나름대로 최선을 다 하려 했지만 지금 다시 돌이켜보면 '내가 정말 최선을 다 한게 맞을까?'라는 생각이 문득 들게 됩니다.<br>

개인적인 노력과는 별개로 팀 전체적인 관점에서 최선이 아니었다라는 생각이 드는데, 아무래도 기능 구현에 개인적인 욕심이 과한 것 같았다는 생각 때문인 것 같습니다.<br>

프로젝트 경험이 없었기에 팀원들에게 폐를 끼치지 않고자 노력했고, 분명 팀 전체에 도움을 줄 수 있도록 노력하자는 마음을 가졌던 것은 사실이지만 막상 기능을 구현하는 과정에서 조금만 더 개선하고자 하는 욕심에 댓글 구현 부분을 아직 배움이 부족한 Ajax와 jQuery를 활용하려고 했었고, 꼭 필요한 기능이 아니었음에도 예상보다 많은 시간을 소비하게 되었습니다.<br>더군다나 시간적 여유가 없다보니 마음은 더욱 급해지고, 다른 팀원들에게 도움을 받을 생각도 못 한 채 눈 앞의 코드에만 온 신경을 쏟았던 것 같습니다.<br>다행히도 해당 기능 구현에 투자한 시간으로 프로젝트 전체에 크게 지장이 생기는 일은 벌어지지 않았지만 시간적 여유가 없는 상황에 욕심을 부린 것 같아 팀원들에게 미안함 마음을 가지고 있었고, 그런 상황에서도 되려 격려와 응원을 아끼지 않은 팀원들에게 감사함을 느끼고 있습니다.<br>

<br>

그리고 세미 프로젝트 진행까지 길지 않은 시간이었다고 생각하는데, 이렇게까지 성장할 수 있게끔 열정적으로 가르침을 주신 강사님께도 정말 감사한 마음입니다.<br>

<br>

---

> ## 앞으로

이번 프로젝트를 통해 개발 뿐만이 아니라 많은 방면에서 성장을 한 것 같습니다.<br>이번에 배운 것들을 토대로 다음 프로젝트에서는 보다 여유를 갖고 팀 전체에 도움이 될 수 있도록 노력하고자 합니다.

<br>







---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
