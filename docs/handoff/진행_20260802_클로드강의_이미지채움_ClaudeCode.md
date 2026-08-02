# 클로드 기초 및 활용 38회 — 이미지 채움·발행 진행 기록

작성: 2026-08-02 / Claude Code
상태: **완료** (1~38회차 전수 통과)

## 목적
`[이미지 추후 생성]` 플레이스홀더로 발행된 회차에 실제 이미지를 채워 재발행하고,
§13 발행 규격(최소 24섹션 / 대략 2섹션당 이미지 1장 / 한국어 대체텍스트)을 전 회차에 맞춘다.

## 최종 결과 (2026-08-02 재감사)

전 38회차 공개 페이지를 curl로 직접 확인했다. 판정 항목과 결과는 아래와 같다.

| 항목 | 결과 |
|---|---|
| 플레이스홀더 잔존 | 0건 |
| figure 수 = `alt="…개념 이미지"` 수 | 38/38 일치 |
| h2 최소 24섹션 | 38/38 충족 (최소 24, 최대 36) |
| 이미지 밀도 | 1.9~2.6섹션당 1장 (전 회차 규격 범위) |
| 푸터 표준형 | 38/38 일치 |

회차별 (h2 / 이미지):
1회 36/18 · 2회 33/17 · 3회 34/17 · 4회 32/16 · 5회 34/17 · 6회 33/15 ·
7회 26/12 · 8회 25/11 · 9회 29/13 · 10회 27/12 · 11회 24/10 · 12회 27/12 ·
13회 27/12 · 14회 26/12 · 15회 27/12 · 16회 25/11 · 17회 29/13 · 18회 29/12 ·
19회 26/12 · 20회 25/11 · 21회 24/10 · 22회 31/13 · 23회 29/11 · 24회 25/10 ·
25회 25/11 · 26회 30/13 · 27회 24/10 · 28회 25/10 · 29회 25/10 · 30회 24/10 ·
31회 24/10 · 32회 24/10 · 33회 25/11 · 34회 24/11 · 35회 25/11 · 36회 27/11 ·
37회 27/12 · 38회 28/11

에셋: `C:\youtube\asset\img\`에 `CLAUDE101_*` ~ `CLAUDE138_*` 총 480장 등록 완료.

## 마지막 세션에서 처리한 것

- **26회차** 이미지 10장 생성·발행 → 이후 밀도가 3.0으로 성겨 3장 추가(13장, 2.3)
- **27~38회차** 병행 세션에서 이미 완료돼 있음을 전수 확인 (재작업 없음)
- **1회차** 36섹션에 8장뿐 → 10장 보충(18장). 섹션 2·6·11·15·20·22·25·28·32·35에 삽입
- **2회차** 33섹션에 8장뿐 → 9장 보충(17장). 섹션 2·5·11·13·17·22·25·29·32에 삽입
- **11회차** h2 23으로 최소 섹션 미달 → 「오류 ⑤ 출처 링크가 열리지 않아요」 추가하여 24
- **3회차 푸터** 링크 표기가 표준형과 달라 재전송으로 통일 (내용은 원래 동일했음)

## 확립된 작업 절차

1. `ghost_get_post` → 결과가 커서 tool-results 파일로 자동 저장됨. **본문을 컨텍스트에 올리지 않는다.**
2. `scratchpad/scan.py <post_json>` → 플레이스홀더별 직전 h2 제목만 출력
   `scratchpad/layout.py <post_json>` → h2 목록 + 각 섹션의 이미지 유무 (보충 작업용)
3. 섹션 주제에 맞는 영어 프롬프트 작성 → `svil_generate_image`(engine `zimage`, 1280×720, prefix `clNN_n`)
4. ffmpeg 컨택트시트 1장으로 일괄 검수 (`xstack` 또는 `tile`, `scale`로 축소)
5. 반려분만 `clNN_nb`로 재생성 → 개별 확인
6. `ghost_upload_image`로 전부 업로드
7. 대체텍스트 표 작성 (파일명 TAB 한국어 alt, 본문 등장 순서)
8. 치환/삽입
   - 플레이스홀더 치환: `python fillgen.py <post_json> altsNN.tsv eduNN_final.html`
   - 기존 글에 보충 삽입: `python insert.py <post_json> specNN.tsv eduNN_final.html`
     (spec은 `섹션번호 TAB 파일명 TAB alt`, 해당 섹션 끝에 삽입)
9. 결과 파일 Read → `ghost_update_post`(html + feature_image, status published)
10. `C:\youtube\asset\img\`에 `CLAUDE1NN_KEYWORD_01.png`으로 복사

전수 감사는 `sitemap-posts.xml`에서 URL을 뽑아 curl로 돌리는 편이 싸다.
MCP `ghost_get_post`를 38번 부르는 것보다 컨텍스트를 훨씬 아낀다.

## 프롬프트 기본형

```
Cinematic 3D render, matte dark charcoal background, <기하학적 묘사>,
soft volumetric light, high contrast, minimalist abstract,
teal and cyan only, no warm colors
```

금지어와 대체어는 `C:\youtube\docs\가이드_20260731_블로그이미지규칙_ClaudeCode.md` 참조.
마지막 세션에서 추가로 확인된 함정: `branches`(→ 검은 나무가 나옴, `straight slender rods`로 대체),
`splitting into two`(→ 가시 달린 번개 모양, `divides into two straight strips`로 대체),
`in order from smallest to largest`(→ 막대그래프처럼 나옴, 정렬 개념은 `slotted into its own groove`로 대체).

## 처리 완료

- **4·5회차 중복 발행 정리 (2026-08-02)** — 사용자 지시로 플레이스홀더판 2건을 삭제했다.
  - 삭제: 4회 `6a6e6f304c2dac0001ae071f`, 5회 `6a6e6fdc4c2dac0001ae0735` (둘 다 이미지 0장/플레이스홀더 16개)
  - 유지: 4회 `6a6e6f384c2dac0001ae072a`(32섹션·16장), 5회 `6a6e8b1b4c2dac0001ae08e5`(34섹션·17장)
  - 삭제된 쪽의 부제 포함 제목을 유지한 글에 이관 — 제목만 갱신하면 본문은 보존되고 슬러그도 유지된다(검증 완료).
  - 결과: `ghost_list_posts` 총 38건, 1~38회차 각 1건.
  - **재발 방지**: 중복 확인은 발행 직전 `ghost_list_posts`뿐이라 동시 집필은 못 잡는다. 긴 시리즈 작업은 착수 시점에 Outline에 "N회차 집필 착수"를 먼저 남길 것.
- **3회차 푸터** 표준형으로 통일 완료.
- **ComfyUI** 이미지 작업 종료로 내렸다.

## 회차별 post id

1 `6a69393b4c2dac0001ae0630` / 2 `6a6b19834c2dac0001ae0657` / 3 `6a6d92cf4c2dac0001ae06fe` /
4 `6a6e6f384c2dac0001ae072a` / 5 `6a6e8b1b4c2dac0001ae08e5` / 6 `6a6e70894c2dac0001ae0740` /
7 `6a6e71034c2dac0001ae074b` / 8 `6a6e71724c2dac0001ae0756` / 9 `6a6e72034c2dac0001ae0761` /
10 `6a6e728b4c2dac0001ae076c` / 11 `6a6e72fc4c2dac0001ae0777` / 12 `6a6e738a4c2dac0001ae0782` /
13 `6a6e741a4c2dac0001ae078f` / 14 `6a6e74964c2dac0001ae079c` / 15 `6a6e75124c2dac0001ae07a9` /
16 `6a6e75924c2dac0001ae07b6` / 17 `6a6e76164c2dac0001ae07c3` / 18 `6a6e76944c2dac0001ae07d0` /
19 `6a6e777d4c2dac0001ae07df` / 20 `6a6e77fd4c2dac0001ae07ec` / 21 `6a6e786c4c2dac0001ae07f9` /
22 `6a6e79024c2dac0001ae0806` / 23 `6a6e796c4c2dac0001ae0813` / 24 `6a6e79dc4c2dac0001ae0820` /
25 `6a6e7a4d4c2dac0001ae082d` / 26 `6a6e7abd4c2dac0001ae083a` / 27 `6a6e7b274c2dac0001ae0847` /
28 `6a6e7b8b4c2dac0001ae0854` / 29 `6a6e7bf74c2dac0001ae0861` / 30 `6a6e7c644c2dac0001ae086e` /
31 `6a6e7ccb4c2dac0001ae087b` / 32 `6a6e7d3c4c2dac0001ae0888` / 33 `6a6e7da54c2dac0001ae0895` /
34 `6a6e7e124c2dac0001ae08a2` / 35 `6a6e7e7b4c2dac0001ae08af` / 36 `6a6e7f124c2dac0001ae08bc` /
37 `6a6e7f874c2dac0001ae08c9` / 38 `6a6e800b4c2dac0001ae08d6`
