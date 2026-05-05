# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is a 빅데이터분석기사 실기 (Big Data Analyst) exam preparation repository. It contains Jupyter notebook solutions to past exam problems (기출동형 모의고사), focusing on machine learning and statistical analysis.

## Exam Structure

| 유형 | 문제 수 | 배점 | 주요 라이브러리 |
|------|---------|------|----------------|
| 작업형 제1유형 | 3문제 | 각 10점 (총 30점) | pandas, numpy |
| 작업형 제2유형 | 1문제 | 40점 | sklearn, xgboost |
| 작업형 제3유형 | 2문제 | 각 15점 (총 30점) | scipy.stats, statsmodels |

- 총점 100점, 합격선 60점 이상, 유형별 과락 없음
- 제6회부터 작업형 제3유형은 단답형으로 출제

## bda-exam-prep Skill

The `.claude/skills/bda-exam-prep.skill` file provides exam preparation automation. Trigger it with natural language:

| 기능 | 키워드 예시 |
|------|------------|
| 채점 | "채점해줘", "몇 점이야", "점수 확인" |
| 오답 분석 | "오답 분석해줘", "왜 틀렸어", "취약 파트" |
| 요점정리 생성 | "요점정리 시작해줘", "개념 정리", "공부 시작" |
| 모의고사 생성 | "모의고사 만들어줘", "동형 문제", "연습" |
| 학습 현황 | "어디까지 했어", "진도 확인", "복습" |

## 요점정리 자동 탐색 경로

"요점정리 시작해줘" 입력 시 유형별로 아래 파일을 자동으로 읽는다.

| 유형 | 탐색 경로 |
|------|----------|
| 작업형1 (pandas) | `references/Pandas/ch01` ~ `ch08` (8개 파일 전체) |
| 작업형2 (ML) | `references/ML/00_metrics`, `01_regression`, `02_classification`, `03_tree_ensemble` |
| 작업형3 (통계) | `references/ML/06_statistical_test` |

사용자가 파일을 첨부한 경우 첨부 파일을 우선 사용한다.
유형 지정 없으면 1 → 2 → 3 순서로 모두 생성한다.

## Directory Structure

```
past_exam_questions/   # 기출 풀이 노트북 (past_exam_N.ipynb)
part_1/                # 작업형 제1유형 요점정리 노트북 (자동 생성)
part_2/                # 작업형 제2유형 요점정리 노트북 (자동 생성)
part_3/                # 작업형 제3유형 요점정리 노트북 (자동 생성)
agents/                # 스킬 에이전트 결과 저장
  01_grader/           # 채점 결과 JSON (grade_result_YYYYMMDD.json)
  05_mistake_analyzer/ # 오답 분석 결과 JSON (analysis_YYYYMMDD.json)
references/
  Pandas/              # ch01~ch08 pandas 참조 노트북
  ML/                  # 00_metrics~06_statistical_test 머신러닝 참조 노트북
  Python/              # 00~08 파이썬 기초 참조 노트북
dataset_dir.txt        # 데이터셋 URL 목록
```

## Running Notebooks

```bash
jupyter notebook past_exam_questions/
```

To run a specific notebook non-interactively:
```bash
jupyter nbconvert --to notebook --execute past_exam_questions/past_exam_1.ipynb
```

## Dataset Sources

Datasets are loaded directly from GitHub URLs within each notebook.
No local datasets are committed; all data is fetched at runtime via `pd.read_csv(url)`.

### Base URLs

| 출처 | Base URL |
|------|----------|
| 기출문제 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/` |
| ADP 실습 | `https://raw.githubusercontent.com/ADPclass/ADP_book_ver01/main/data/` |

### 파일명 네이밍 규칙

```
{회차}_{유형}_{번호}.csv

작업형1: {회차}_1_1.csv, {회차}_1_2.csv, {회차}_1_3.csv
작업형2: {회차}_2_train.csv, {회차}_2_test.csv
작업형3: {회차}_3_1.csv, {회차}_3_2.csv
```

### 확인된 데이터셋 목록

| 파일명 | 유형 | URL |
|--------|------|-----|
| `10_1_1.csv` | 작업형1 Q1 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_1_1.csv` |
| `10_1_2.csv` | 작업형1 Q2 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_1_2.csv` |
| `10_1_3.csv` | 작업형1 Q3 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_1_3.csv` |
| `10_2_train.csv` | 작업형2 학습 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_2_train.csv` |
| `10_2_test.csv` | 작업형2 평가 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_2_test.csv` |
| `10_3_1.csv` | 작업형3 Q1 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_3_1.csv` |
| `10_3_2.csv` | 작업형3 Q2 | `https://raw.githubusercontent.com/YoungjinBD/data/main/exam/10_3_2.csv` |
| `student_data.csv` | ADP 실습 | `https://raw.githubusercontent.com/ADPclass/ADP_book_ver01/main/data/student_data.csv` |

새 기출 데이터셋 추가 시 위 테이블에 행을 추가한다.

## Notebook Structure

Each past exam notebook (`past_exam_N.ipynb`) follows the exam format:
1. **작업형 제1유형** — pandas/numpy 데이터 처리, 단일 수치 도출
2. **작업형 제2유형** — EDA, 전처리, 모델 학습 (SVM / XGBoost / RandomForest), RMSE/F1 평가, `result.csv` 제출
3. **작업형 제3유형** — 가설검정, 회귀분석, 통계량 도출

Korean font setup for matplotlib appears at the top of each notebook — `Malgun Gothic` is used on Windows.

## Key Libraries

`pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `scipy`, `statsmodels`

## Conventions

- Cells annotated with `# 모름` mark areas where the author was unsure; these are flagged as `UNKNOWN` during grading.
- Target variable for regression problems is typically `grade` or a similar score column.
- Categorical encoding uses manual `.map()` dicts rather than `pd.get_dummies` or `LabelEncoder`.
- `result.csv` in `past_exam_questions/` stores model prediction output for submission (missing file = −10pts in grading).
- Final answer output convention: `print(round(결과, N))`.
