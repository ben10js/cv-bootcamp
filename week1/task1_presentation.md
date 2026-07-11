---
marp: true
theme: default
paginate: true
backgroundColor: #ffffff
style: |
  section {
    font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif;
    font-size: 20px;
    padding: 40px 60px;
  }
  h1 {
    color: #2B5B84;
    font-size: 1.8em;
  }
  h2 {
    color: #333333;
    border-bottom: 2px solid #2B5B84;
    padding-bottom: 8px;
    font-size: 1.35em;
  }
  h3 {
    color: #4A90E2;
    font-size: 1.1em;
    margin-top: 15px;
    margin-bottom: 5px;
  }
  ul {
    font-size: 0.95em;
    line-height: 1.5;
    margin-top: 5px;
  }
  li {
    margin-bottom: 4px;
  }
  .highlight {
    background-color: #FFF3CD;
    padding: 2px 6px;
    border-radius: 4px;
    font-weight: bold;
  }
  footer {
    font-size: 0.5em;
    color: #888888;
  }
---

<!-- _paginate: false -->
<!-- _backgroundColor: #f8f9fa -->

# [1차 업무 보고서]
## Git 활용 코드 관리 및 Computer Vision AI 데이터 전처리

<br>

**주제:** AI 모델 학습을 위한 픽셀 단위 이미지 처리 및 전처리 파이프라인 구축  
**작업 브랜치:** `feature/image-processing` -> `main`  
**제출자:** 코멘토 직무부트캠프 멘티  
**제출일:** 2026년 7월

---

## 1. Git 협업 환경 구축 및 픽셀 단위 색상 감지

### 실무 Git 버전 관리 (Git Workflow)
- **브랜치 전략:** 기본 저장소 생성 후 독립적인 작업 브랜치(`feature/image-processing`) 생성
- **협업 프로세스:** 기능별 분할 Commit -> 원격 저장소 Push -> PR(Pull Request) 생성 및 리뷰 요청

### OpenCV 활용 빨간색 필터링 (`color_filter.py`)
- **BGR -> HSV 색상 공간 변환:** 조명 변화에 강건한 색상 추출을 위해 HSV 공간 활용
- **듀얼 마스킹(Dual Masking) 기법:** 
  - 빨간색 Hue 범위가 0도와 180도 양쪽에 걸쳐 있는 특성 반영
  - 앞구간(0~10)과 뒷구간(170~180) 마스크를 병합(`mask1 + mask2`)하여 추출
- **비트 연산(`bitwise_and`):** 마스크 영역과 원본 이미지를 매핑하여 특정 색상 영역만 정확히 분리

---

## 2. AI 학습용 데이터 전처리 및 증강 파이프라인

### Hugging Face 데이터셋 스트리밍 (`image_preprocessing.py`)
- `ethz/food101` 데이터셋을 실시간 스트리밍(`streaming=True`)으로 로드하여 대용량 다운로드 최적화

### [심화 과제] 이상치 탐지 (Outlier Detection) 알고리즘
1. **어두운 이미지 제거:** Grayscale 변환 후 픽셀 평균 밝기 40 미만 데이터 필터링
2. **작은 객체 이미지 제거:** Canny Edge 및 Contour 탐지를 활용해 객체 면적이 전체 화면의 5% 미만인 데이터 필터링

### AI 표준 전처리 4종 및 데이터 증강
- **크기 조정 및 노이즈 제거:** 표준 AI 입력 규격(224x224) 리사이즈 및 Gaussian Blur(5x5) 노이즈 완화
- **정규화 및 증강:** 0.0 ~ 1.0 float 정규화(Normalize) 및 과적합 방지용 증강(좌우반전, 회전, 색상변화) 적용

---

## 3. 최종 요약 및 실무 적용 성과

### 최종 결과물 요약
- **최종 산출물:** 전처리 및 증강 파이프라인을 최종 통과한 대표 샘플 5장 (`preprocessed_samples/`)
- **문서화:** 파이프라인 아키텍처와 실행 가이드를 정리한 `README.md` 완비
- **협업 연동:** GitHub PR 생성 완료 및 코드 리뷰 대기 상태 (No Merge)

### 실무 적용 의의 및 멘토님 질의
- **의의:** 노이즈 및 이상치 제거를 통한 AI 모델 학습 안정성 확보 및 Git Flow 실무 협업 프로세스 숙달
- **멘토님 질의:** 
  > "이상치 탐지 시 객체 크기 판별을 위해 Contour 면적 비율 5%를 기준으로 잡았습니다. 실제 현업 Computer Vision 프로젝트에서는 대용량 데이터셋의 이상치(배경만 있거나 노이즈가 심한 이미지)를 필터링할 때 보통 어떤 기준이나 자동화 기법을 주로 활용하시는지 궁금합니다."
