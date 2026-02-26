# REVUA 글 발행 절차

## 개요
달비님이 관심 있는 아티클을 → AEO 최적화된 한국어 글로 → revua.kr에 발행하는 표준 절차.

---

## Step 1. 소스 수집 (달비님)
- 관심 있는 아티클 링크 또는 텍스트를 새벽이한테 붙여넣기
- 소스: thecaio.ai, X, 뉴스레터, 해외 블로그 등 무엇이든 OK

---

## Step 2. 예상 질문 리스트 뽑기 (달비님 → ChatGPT/Gemini)
아티클을 ChatGPT 또는 Gemini에 붙여넣고 아래 프롬프트 사용:

```
이 글을 읽고 한국 독자가 AI/ChatGPT/OpenClaw에 대해 가질 만한
예상 질문 10개를 뽑아줘. 질문 형태로, 구체적으로.
```

뽑은 질문 리스트를 새벽이한테 전달.

---

## Step 3. 제목 결정 (새벽이)
- 질문 리스트 → AEO 최적화 제목으로 변환
- 제목 유형에 따라 다르게:
  - 가이드/튜토리얼 → 정보성 스타일 유지 ("OpenClaw 설치 방법", "무료로 실행하는 방법")
  - 비교/평가/후기 → 구어체로 ("OpenClaw vs ChatGPT Plus, 어떤 게 더 편할까요?", "OpenClaw 쓸만할까요?")
- 번역투 제목 절대 금지
  - "~있습니까?" → "~있나요?"
  - "~무엇인가요?" → "~뭐예요?" or "~뭔지 정리"
  - 나쁜 예: "OpenClaw가 가치가 있습니까?"
  - 좋은 예: "OpenClaw 쓸만할까요? 유료 결제 전 필수 체크사항"

---

## Step 4. 번역 + 리라이팅 (새벽이)

### 번역 규칙
- 전문 번역 (요약 금지, 원문 전체)
- 자연스러운 한국어 (번역투 금지)
- "OpenClaw는/를/가" (받침 없음)
- em dash(—) 절대 사용 금지 → 쉼표/마침표로 대체
- 초성(ㅋㅋ 등) 사용 금지
- "있습니까?" → "있나요?", "합니까?" → "할까요?"

### 금지 단어
흐름, 흘러, 성장, 정리, 성공, 공유, 습관, 갈아엎다, 소통, 꽂히는,
드러나다, 하루를 갉아먹다, 시선이 한쪽으로 모입니다

### HTML 처리
- h2, h3, p, ul, li, a 태그 유지
- 본문 맨 앞 h1 제거 (WordPress가 따로 렌더링)
- 본문 맨 앞 excerpt 단락 제거 (WordPress가 따로 렌더링)
- 날짜/작성자 메타 단락 제거
- 본문 내 텍스트 목차 제거 ("이 가이드에 포함된 내용" h2 + ul 섹션)

---

## Step 5. DB 삽입 (새벽이)

### 접속 정보
```python
import pymysql
conn = pymysql.connect(
    host='172.18.0.2',
    user='revua',
    password='revua_db_2026!',
    database='wordpress',
    charset='utf8mb4'
)
```

### 삽입 항목
- post_title: 번역된 제목
- post_content: 번역된 본문 HTML
- post_excerpt: 첫 단락 요약 2~3문장
- post_status: 'publish'
- post_type: 'post'
- post_name: 영문 slug (원본 URL slug)
- post_date / post_date_gmt: 발행일
- comment_status: 'closed'
- ping_status: 'closed'

### 카테고리 연결
- wp_term_relationships에 INSERT
- term_taxonomy_id=11 (openclaw 카테고리)

---

## Step 6. Schema Markup 추가 (새벽이)
AEO Layer 5 적용. 글 유형에 따라:
- 가이드/튜토리얼 → HowTo schema
- 비교글 → FAQPage schema
- 일반 정보글 → Article schema

functions.php에 post_id별 schema 삽입 또는 Rank Math 설정.

---

## Step 7. 검수 (새벽이 → 달비님)
- 발행된 URL 확인
- 본문 깨짐 없는지
- 목차 정상 작동 여부
- 달비님 최종 확인 후 완료

---

## 2차 목표: 달비님 직접 편집
- GitHub revua-posts repo에 .md 파일로 원본/편집본 보관
- 편집 이력 → 3차 few-shot 데이터로 활용

---

## 카테고리 분류
| 카테고리 | term_taxonomy_id | 대상 |
|---|---|---|
| openclaw | 11 | OpenClaw 관련 전체 |

---

## 우선순위
1. OpenClaw 관련 43개
2. Claude Code 관련 42개
3. Industry Guide 10개 (한국 로컬라이징 별도 작업)
4. 기타 14개
