---
title: Jupyter Notebook을 쓰지 않게 되는 몇 가지 이유에 대해서
tags: [jupyter]
category: 잡담
layout: post
---

Jupyter Notebook이 참 좋은데 참 별로란 말이죠?

<!--more-->


## 🛴 **들어가며**

2015년 말부터 데이터에 대해 공부하며 처음 쓰기 시작한 언어는 R이었습니다. 당시를 떠올려보면 데이터 분석은 곧 R을 의미했었습니다. 많은 책들이 쏟아져 나오고 있었고, 워드클라우드를 그릴 줄 아는 것이 기본 소양이었습니다. 공부를 더 하다보니 캐글을 접하게 되고, 많은 사람들이 파이썬을 쓰고 있는 것을 알게 되었습니다. 특히나 **"R vs 파이썬"**은 재미있는 주제 중 하나였습니다.
결국 저는 2017년을 기점으로 파이썬만을 사용하기 시작했습니다. 훨씬 구조적이고 깔끔한 코드를 작성하는 것이 가능했기 때문입니다. 지금 다니는 회사에 입사하였을 때 분석 조직은 R을 썼기 때문에 어쩔 수 없이 한동안은 R을 썼지만 기회만 되면 파이썬으로 업무를 진행하려 했었습니다. 2019년 말부턴 딥러닝이 업무에 사용되면서 조직 전체적인 분위기가 **'파이썬을 사용해야 한다'**로 기울었죠.

분석 업무는 많은 부분이 실험이고 빠르게 프로토타입을 만드는 것이 중요하기 때문에 Jupyter Notebook을 자주 사용하게 되었습니다. 하지만 분석 조직이 성숙해지며 ML 모델을 운영하는 케이스가 생겨나기 시작하고 프로덕션을 위한 분석 프로젝트나 ML 프로젝트가 늘어나기 시작했습니다. 문제는 지금부터였습니다. 기본적인 분석 업무나 ML 모델링을 Jupyter Notebook으로 진행하였는데 프로덕션 코드를 작성하다보니 Jupyter Notebook이 불편한 부분이 제법 많았습니다. 빠르게 무언가를 확인해볼 수 있다는 장점 하나로 사용하고 있었지만, 이런 부분이 마음에 안들었습니다.

- **코드를 중구난방으로 작성하게 됨**
: 특히 다른 분들이 작성한 코드를 볼 때 이 문제는 더욱 치명적으로 다가왔습니다. 정말 읽기 힘들더라구요.
- **버전 관리가 어려움**
: 개인적으로 제가 생각하는 Jupyter Notebook의 최고 단점입니다. 사실상 Jupyter Notebook 파일은 JSON 파일이기 때문에 수정된 부분을 찾기가 굉장히 어렵습니다.
- **개발이 느려짐**
: 전 VS Code를 이용해 개발 업무를 진행하는데 편리한 Extension이나 빠른 자동 완성 기능을 사용할 수 없어서 너무 불편했습니다.

그래서 결국 Jupyter Notebook은 빠른 프로토타이핑만을 위해 사용하고 실제 실험 코드나 프로덕션용 코드는 모두 VS Code를 이용해서 패키징하여 개발하고 있습니다. 갑자기 이런 이야기를 길게 하게 만든 것은 Towards Data Science에 올라온 [한 아티클](https://towardsdatascience.com/3-reasons-why-jupyter-notebook-is-steering-your-team-the-wrong-way-abb53cc46823)입니다. 저와 비슷한 생각을 갖고 작성된 글이어서 무슨 이야기를 하는지 살펴보며 Jupyter Notebook이 어떻게 분석 조직을 잘못된 방향으로 가게 만드는 지 살펴보겠습니다.

## 💣 Jupyter Notebook은 어떤 문제가 있길래?

저자는 지난 몇 년간 단순한 ML 모델 적용을 넘어서는 무언가가 필요하다는 것을 깨닫기 시작하면서부터 MLOps 사례들이 등장했다고 합니다. 실제로 제가 재직 중인 회사에서도 분석 조직이 생긴지는 오래 되었지만 MLOps에 대한 논의는 올해 처음인 것 같구요.

MLOps가 도입되면 모델 개발보다 복잡한 요소들 (재현 가능한 코드, 모니터링 등)이 많이 추가됩니다. 따라서 저자는 MLOps를 성공적으로 도입하기 위해서는 개발에서 배포로 넘어가는 과정이 부드럽게 이어져야 하며, 이런 Seamless함은 소스 코드의 높은 퀄리티로부터 나온다고 말합니다. 실제로 프로젝트를 하다보면 개발과 배포를 별도로 생각하는 분들이 다수 있었습니다. 우선 빨리 뭔가를 만들고, 이것을 정리해서 배포용 코드를 작성하면 된다고 생각하는 경우인데 결국 코드의 퀄리티는 떨어지고 중간중간 새로운 Feature를 추가할 때마다 코드 베이스가 뒤집어지는 일을 경험하게 되더군요. 그야말로 사상누각(砂上樓閣)이었습니다.

Jupyter Notebook은 많은 DS 팀에서 채택하고 있는 도구입니다. 빠르게 데이터를 살펴보기에 용이하고, 데이터 전처리나 모델링을 빠르게 테스트해볼 수 있기 때문이죠. 데이터 사이언티스트에게 정말 편한 도구이지만, 위에서 말한 높은 퀄리티의 소스 코드를 작성하는 데에 올바른 도구가 아니라고 저자는 이야기합니다.

### 🧶 나쁜 코딩 습관을 만듦

<div align="center" markdown="1">
  ![](/assets/images/2021-10-27-why-i-dont-use-jupyter-notebook/bad_code_meme.png)
</div>

소프트웨어 개발에 있어 모범적인 방식 중 하나는 소스 코드를 여러 개의 파일로 분리해놓는 것입니다. 협업하기도 편하고 디버깅과 유지보수도 훨씬 쉬워집니다. 하지만 Jupyter Notebook을 사용한다면 거의 불가능에 가깝습니다. 모든 코드가 하나의 노트북 파일에 다 담겨지기 때문입니다.

보통 Juypter Notebook에 코드를 작성하는 방식을 생각해본다면 단일 셀 안의 코드는 길어지고, 동일한 로직의 코드는 여기저기 복사되어 사용됩니다. 그러다보면 코드를 작성한 사람조차도 원하는 로직을 찾기 위해서 노트북 파일의 처음부터 끝까지 다 찾아봐야 하는 불상사가 발생합니다. 또 무언가를 실행하기 위해서 셀을 위아래로 탐색해가며 실행하고 불필요한 시간을 소모하게 되죠.

결국 코드를 유지보수하기는 어려워지고, 최악의 경우에는 코드를 처음부터 다시 깔끔하게 작성하게 됩니다. 단순히 노트북 파일에 코드를 작성하는 것이 빠르게 무언가를 할 수 있다는 이유로 중구난방으로 습관이 들게 된다면 나중에 실제 프로덕션용 코드를 작성할 때 그 습관을 고치기 어렵게 됩니다.

### 🔒 버전 관리가 거의 불가능함

<div align="center" markdown="1">
  ![](/assets/images/2021-10-27-why-i-dont-use-jupyter-notebook/version_control_meme.png)
</div>

개발의 규모가 커지고 협업하는 인원이 늘어나기 시작하면 버전 관리는 필수가 됩니다. 하지만 Jupyter Notebook은 버전 관리가 거의 불가능한 도구입니다. 하나의 노트북 파일은 소스 코드를 포함하는 거대한 JSON 파일입니다. 그러다보니 Jupyter Notebook 파일의 버전을 관리한다는 것은 불가능합니다.

- 두 버전의 차이점을 확인하기 어려움.
- 서로 다른 버전의 파일을 Merge하는 것이 불가능함.
- Jupyter Notebook 파일은 내부에 메타데이터가 포함되기 때문에 변경 이력을 올바르게 확인하기 어려움.
예를 들어서 노트북 파일을 실행하는 순서가 바뀌거나 내부의 시각화 결과가 바뀐다면 코드는 동일하나 서로 다른 파일이 되어 버림.

### 🤷‍♂️ 너무 좁은 생태계

<div align="center" markdown="1">
  ![](/assets/images/2021-10-27-why-i-dont-use-jupyter-notebook/right_tool_meme.png)
</div>

VS Code는 코드 작성을 편하게 만들어주는 수많은 Extension들로 호평을 받습니다. Linting도 되고 디버깅도 되고 그야말로 안되는 것이 없습니다. 하지만 Jupyter Notebook 환경을 생각해보면 자동완성을 제외하면 기능이 거의 없습니다. 물론 `nbextension` 등을 설치하거나 JupyterLab을 사용하면 생산성을 높일 수는 있으나 여전히 추가적인 기능이 필요합니다. 

전 최근에 와서는 VS Code에서 Extension을 설치해 Jupyter Notebook 파일을 불러와서 작업하게 되었습니다. 하지만 Linting 기능도 버그가 많고 디버깅이 쉽지 않아서 결국은 Jupyter Notebook 자체를 멀리하게 되었습니다.

## 🙏 마무리하며

Jupyter Notebook은 데이터 사이언티스트에게 필수적인 도구라는 것은 부인할 수 없습니다. 여전히 훌륭한 도구이며, 그 어떤 도구보다 단순한 작업을 손쉽게 해주니까요. 하지만 ML 프로젝트의 대부분이 퀄리티 높은 코드를 요구하게 되면서 단점이 서서히 부각되고 있습니다. 데이터 사이언티스트들은 다른 데이터 사이언티스트와 협업과 복잡한 로직을 처리하기 위해서 결국 퀄리티 높은 코드를 작성해야 합니다. 

본 블로그 글을 통해 현업에서 일하며 느꼈던 답답함을 풀어주는 아티클이 있다고 생각이 들어 소개드렸습니다. 하지만 아직 제가 속해 있는 조직은 여전히 Jupyter Notebook을 많이 사용하고 있습니다. 서로 다른 `.ipynb` 파일을 주고 받으며 서로의 코드를 읽는 데에만 많은 시간을 소모하고 있습니다. 제가 리딩하는 프로젝트라면 처음부터 Jupyter Notebook은 간단한 작업만을 위해 사용하는 것으로 못박으면 되지만, 반대로 제가 단순히 팀원으로 참여하는 프로젝트라면 여전히 어려움이 있을 것 같습니다. 만약 이 글을 보고 계신 다른 분께서 저와 같은 상황에 놓여있으시다면 짧게라도 이 문제에 대해 고민해보는 시간을 가져보시는 것은 어떨까요?