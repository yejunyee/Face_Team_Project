# 얼굴  인식 프로젝트

------------------------

### 팀원

이예준, 이시원, 지영수

------------------------

### 업무분담
이예준: 연구 스케줄 관리, 연구 구현

이시원: 발표정리

지영수: 자료조사, 연구 구현

------------------------

### 과제요약
처음에는 퍼스널 컬러에 대한 연구를 진행하다가 자료부족과 우리들의 기술적인 역량 문제로
시간안에 해결하지 못할 것으로 생각하여 사람의 얼굴 특정값과 오픈소스를 활용해 보다 더 간단하면서도
쉽게 접근할 수 있는 얼굴 감정 인식을 연구해보기로 했다.

------------------------

### 일정계획
11/16 주제 선정 및 기타 회의
~11/20 연구 구현 및 수정
11/25 연구실패
11/25~12/7 새로운 연구 자료조사
12/8~12/11 구현 및 보고서 작성

------------------------

### 코드 요약
1. index.html
<pre>
<code>
<title>Document</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    canvas {
      position: absolute;
    }
  </style>
</code>
</pre>
웹 서버를 통해 쉽게 접근하면서 출력되는 사이즈를 html언어로 활용해 조절하였고
<pre>
<code>
<script defer src="face-api.min.js"></script>
  <script defer src="script.js"></script>
</code>
</pre>
실행 가능한 코드들을 포함시켰다.

2. javascript
<pre>
<code>
Promise.all([
  faceapi.nets.tinyFaceDetector.loadFromUri('/models'),
  faceapi.nets.faceLandmark68Net.loadFromUri('/models'),
  faceapi.nets.faceRecognitionNet.loadFromUri('/models'),
  faceapi.nets.faceExpressionNet.loadFromUri('/models')
]).then(startVideo)
</code>
</pre>
얼굴에서 알고 싶은 것을 결정하고 사용 가능한 모델들을 사용하여 원하는 기술을 구현하게끔 만듬
<pre>
<code>
video.addEventListener('play', () => {
  const canvas = faceapi.createCanvasFromMedia(video)
  document.body.append(canvas)
  const displaySize = { width: video.width, height: video.height }
  faceapi.matchDimensions(canvas, displaySize)
</code>
</pre>
faceapi로 추출할 거를 비디오로 연결하고 캔버스를 만들어 거기에 출력되게 만들고
출력크기를 조절하였다.
<pre>
<code>
setInterval(async () => {
    const detections = await faceapi.detectAllFaces(video, new faceapi.TinyFaceDetectorOptions()).withFaceLandmarks().withFaceExpressions()
    const resizedDetections = faceapi.resizeResults(detections, displaySize)
    canvas.getContext('2d').clearRect(0, 0, canvas.width, canvas.height)
    faceapi.draw.drawDetections(canvas, resizedDetections)
</code>
</pre>
face.api를 이용해서 얼굴을 인식하게 만들었다 여기서 우리가 구현할려는 기술의 핵심 코드가 있는데
<pre>
<code>
faceapi.draw.drawFaceLandmarks(canvas, resizedDetections)
    faceapi.draw.drawFaceExpressions(canvas, resizedDetections)
  }, 100)
</code>
</pre>
face.api로 68개의 포인트를 감지하는 얼굴 랜드마크를 계산하고
인식된 얼굴 표정을 인식해주는 api와 연결하여 원하는 결과를 이끌어냈다.

-----------------------

### 예상결과
사람의 얼굴이 인식되고 그 인식된 얼굴의 표정을 읽고 무슨 감정인지 출력된다.



https://user-images.githubusercontent.com/62829568/206907514-7d75b64f-8e42-42c5-94bc-f39c4532e4c8.mp4



------------------------

### 참고문헌
	(초보자를 위한) OpenCV를 이용한 영상처리 / 임동훈 저
  https://cafe.naver.com/imgprocafe
  
------------------------

### 2022년 11월 16일 회의록
지영수: 얼굴의 이목구비를 읽어서 관상 어플을 만들고 싶다

이시원: 표정을 읽어서 화남 평온 기쁨 같은 것들을 캐치해서 배경의 명도를 조절하는 시스템

이예준: 얼굴의 색깔을 읽어서 그 사람과 잘 어울리는 컬러의 옷을 알려주는 것(퍼스널컬러)

이 중에서 우리가 할 수 있는 선에서 고민을 해보다가 팀장 이예준의 주제를 연구하기로 했다
그 이유는 팀원 지영수, 이시원의 주제도 너무 좋지만,
우리가 배운 내용 중에서 제한시간 안에 잘 구현하고 상업적으로도
활용하기 좋은 주제가 뭔지에 대해 초점을 두고 의논해봤는데,
팀장의 주제가 가장 잘 부합하다고 결론이 나왔다.

2022년 11월 25일 회의록
지영수: 퍼스널 컬러말고 조금 더 가볍게 접근해서 연구를 진행해보자

이예준: 생각보다 주제가 어렵고 자료도 부족해서 연구가 어려웠다.
쉽게 응용하기 좋은 오픈소스들을 활용해 기술을 구현해보는 건 어떨까?

이시원: 모두의 의견에 공감했고 주제를 바꿔서 좀 더 우리가 잘 할수 있는걸로 변경하는 것이 좋다 생각

------------------------

### 소감문
이예준: 처음에 퍼스널 컬러라는 주제로 팀원들과 열심히 연구를 해봤지만 당장은 주어진 시간과 우리들의 역량으로는 구현하기
힘들다 생각이 들어 얼굴 인식 기술의 정수에 더 가까우면서 쉽게 활용하고 접근하기 좋은 기술 중에 뭐가 있을까
생각해보다가 팀원들끼리의 회의 끝에 얼굴 표정 인식 연구를 시작하게 됐습니다.
오픈소스를 활용해 사람의 표정에서 나오는 특수한 값을 이용해 표정을 읽어 출력하게 되는 연구를 진행하게 됐는데,
이 얼굴인식이 정말 다양하게 일상생활에서 쓰일 수 있겠구나라는 생각이 들었고 앞으로 이 수업 말고도 더 연구해보고 싶은 생각이 들었다.

지영수: 처음 팀프로젝트를 구상할 때, 퍼스널컬러를 찾아주는 프로그램을 구현해보기로 했지만, 퍼스널컬러의 유래가 우리나라에서 나왔고 비교적 최근의 주제라 자료가 많이 없어 부득이하게 주제를 얼굴을 인식하여 감정읽기로 바꾸게 되었습니다. 처음 골랐던 주제가 무산되어 시간이 촉박해 정신없이 자료 조사를 했던 것 같습니다. 퍼스널 컬러라는 좋은 주제를 놓친것이 아쉽지만, 사람의 감정을 표정으로 읽어내는 좋은 오픈 소스가 있어 재밌게 팀프로젝트를 마쳤습니다. 비록 이번에는 능력이 부족하여 퍼스널 컬러를 포기했지만 다음 또 팀프로젝트가 있다면 다시 한번 도전하여 구현해보고 싶습니다.

이시원: 처음에는 우리가 얼굴인식과 퍼스널 컬러 선정이라는 
두 가지 기술을 함께 구현할수 있을까 라는 생각이 많이 들었습니다. 
하지만 적극적이고 항상 우리에게 도움을 주려하시는 교수님, 
그리고 훌륭한 실력을 가진 팀원과 함께한다면 충분히 할수 있을 것이라는 생각이 들었다.

-------------------------

### 리플릿
![KakaoTalk_20221212_160220427_01](https://user-images.githubusercontent.com/62829568/208410448-d8be2935-077d-4354-a5aa-144f5796a99d.png)
![KakaoTalk_20221212_160220427](https://user-images.githubusercontent.com/62829568/208410463-94851745-61d5-4cc5-9078-6a3e91dc3417.png)

