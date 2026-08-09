## 대상

- 프로젝트: 블로그파이프라인 (blogger_images) / 테마·인프라는 `C:\Projects\ghost`
- 작업 폴더: `C:\Projects\blogger_images\.claude\worktrees\ai-news-ghost-post-4347cb`
- 세션 시각: 2026-08-07 20:10 (KST)

## 세션 요약

Ghost 블로그의 카테고리 체계를 유튜브 재생목록으로 통일하고(그 과정에서 홈 섹션 두 개를 비운 사고와 복구 포함), 없던 방문자 통계를 신설했다. ComfyUI가 조용히 NaN을 뱉던 문제의 근본 원인을 찾아 미완성 이미지 27장을 채웠고, 저시력자 PC 세팅 글을 3부작으로 확장 발행했다.

## 완료된 작업

### 1. 소버린 AI 심층분석 발행

post id `6a73ce614c2dac0001ae0a02` · 30섹션 · 이미지 15장 · 출처 10건
축: 김유원 대표 2025-04 발언이 자기 탈락 사유가 된 경위 / 같은 코사인 유사도로 네이버는 탈락·업스테이지는 제기자가 사과한 대조 / 2차 평가 4파전.

### 2. 🔴 Ghost 카테고리를 유튜브 재생목록으로 통일 — 사고 있었음

**사고**: 태그 이름만 보고 중복 태그를 정리하며 빈 껍데기 5개를 삭제했는데, 홈(`theme/svil/index.hbs`)이 태그 이름이 아니라 **slug 기반 계단식 배제 필터**였다. `ainyuseu`·`weokeupeulrou` 삭제로 **AI 뉴스·워크플로우 섹션이 통째로 비었다.** 원인은 **태그 목록만 보고 홈을 한 번도 열어보지 않은 것.** 저장소 `C:\Projects\ghost`에 정본이 있었는데 그것도 안 봤다. 복구는 새 태그의 slug를 원래 값으로 재지정.

**재설계**: 분류 체계가 둘(재생목록 8 vs 홈 섹션 6)이라 한쪽을 건드리면 다른 쪽이 깨지는 구조였다. 재생목록 하나로 통일.

| 홈 순서 | 카테고리 = 태그 이름 | slug |
|---|---|---|
| 1 | AI 뉴스 | `ainyuseu` |
| 2 | 심층분석 | `simceungbunseog` |
| 3 | 클로드 기초 및 활용 | `keulrodeu-gico-mic-hwalyong` |
| 4 | AI 기초 및 활용 | `ai-gico-mic-hwalyong` |
| 5 | AI 활용 & 개발 워크플로우 | `weokeupeulrou` |
| 6 | SVIL 앱 소개 | `svil-aeb-sogae` |
| 7 | 저시력 접근성 이야기 | `jeosiryeog-jeobgeunseong-iyagi` |
| 8 | 과학 & SVIL 연구노트 | `gwahag-svil-yeongunoteu` |
| — | 기타(fallback) | `etc` |

분류 우선순위: 심층분석 > 클로드 > AI기초 > 앱소개 > 저시력 > 과학 > 워크플로우 > 뉴스
별칭 태그(`강의` `AI교육` `접근성` `edu` `info`) 폐기. 내부 태그 `#hidden` 도입 — 정책 페이지·본문 없는 글을 홈에서만 감추고 URL은 유지(앱스토어 등록 개인정보처리방침 주소 보호).

`scripts/sync-section-tags.js` 버그 2건 수정 — `limit=all`이 100건에서 잘려 144편 중 100편만 처리되던 문제, slug 자동 교정 방어선 추가.
브랜치 `feat/home-sections-youtube-playlists` (`7f3609b`), gscan 통과, `npm run theme:deploy` 배포 완료.

### 3. 7월 이전 발행분 정리

2025-12-18~2026-06-19 발행 20편 draft 전환 → 저시력 접근성 섹션이 0편이 되어 해당 3편(PC 세팅·코멧 브라우저·접근성 칼럼) 복구. 최종 draft 17편.
본문 없는 draft 4편은 **원문 JSON 보관 후 삭제**(`ghost/docs/archive/`, `4be45e8`).

### 4. Umami 방문자 통계 신설

그동안 수집 자체가 없었다(Ghost 6.56이나 Tinybird 미연동, GA·코드인젝션 없음, 트래킹 스크립트 0개). **과거 방문 기록은 존재하지 않는다.**

- 대시보드: https://umami-production-9dba.up.railway.app
- Railway 서비스 `umami` — DB는 기존 `mysql`의 `umami` 스키마 재사용(서비스 추가 1개로 억제)
- website id `3c18ee24-731f-4cd9-ac5a-4f2da3f05c4a`
- 추적 스크립트는 **테마**에 있음 (`theme/svil/default.hbs`). Ghost 코드인젝션 설정은 통합 토큰 403이라 테마 경로 사용
- 조회: `cd /c/Projects/ghost && npm run stats` — **Umami 로그인 없이 MySQL 직접 조회**, 토큰 불필요
- 관리자 비밀번호 난수 재설정 완료(`scripts/umami-set-password.js`, `1c50471`). 값은 Railway `umami` 서비스 변수 `UMAMI_ADMIN_PASSWORD` + `C:\Projects\ghost\.env.umami.local`(gitignored)
- 수집 시작 2026-08-07

### 5. 🔴 ComfyUI NaN 근본 원인 규명

검정(4KB)·컬러 노이즈(1.7MB대) 이미지는 프롬프트·드라이버 문제가 아니라 **VRAM 부족**이다.

ComfyUI가 Z-Image를 fp8 `5,869MB` 또는 bf16 `11,738MB`로 로드하는데, bf16을 골라 놓고 실제로 못 올리면 VAE 디코드가 NaN을 뱉는다. **에러가 안 난다** — 샘플러는 8/8 완주하고 `Prompt executed`까지 찍히며 로그엔 `nodes.py:1682: RuntimeWarning: invalid value encountered in cast` 한 줄뿐.

- 판별: 파일 크기 (~4KB 검정 / 1.5MB↑ 노이즈 / 600KB~1.1MB 정상)
- 대처: `svil_comfy_free`로는 안 됨(실측 회수 0.27GB, 심지어 −0.17GB). **프로세스 재기동**이 유일. 여유 15GB 이상 확보 후 시작
- **병렬 큐잉 금지**: 한 메시지에서 여러 장 동시 호출 시 전부 타임아웃 + 큐 미등록 + VRAM 락 좀비(실측 `held_sec 645`). `svil_vram_force_release`로 해제

가이드 `C:\youtube\docs\가이드_20260731_블로그이미지규칙_ClaudeCode.md`에 절 신설.

### 6. 이미지 27장 채움

소버린 AI(h2 30/figure 15) · TXTDrop(h2 26/figure 12), 홀더 0. 반려·재생성 8장(`stream`이 실사 물줄기 사진을 부른 게 2건 등).
에셋 `SOVEREIGNAI_01~15`, `TXTDROP_01~12`.

### 7. 저시력자 PC 세팅 3부작

기존 「간단 가이드 #1」(5섹션·2,340자)을 3편 확장, 원글 draft 전환.

| 편 | 제목 | 섹션/이미지 | post id |
|---|---|---|---|
| 1 보기 | 화면을 보이게 만들기 | 35/16 | `6a759478f3fc0b00019ea92d` |
| 2 조작 | 손이 눈을 대신하게 | 29/12 | `6a75af1df3fc0b00019ea945` |
| 3 정리 | 찾는 시간을 아예 없애기 | 26/11 | `6a75b1def3fc0b00019ea957` |

합계 약 25,000자, 상호 링크 완료. 축은 "눈을 아껴 쓰는 것".
사실검증: Windows 11 Pro 빌드 26200(ko-KR) 레지스트리 직접 조회(`ScreenMagnifier` 확대증분 50%, `ColorFiltering` HotkeyEnabled 1) + MS 한국어 공식 문서 교차 확인.
이미지 39장, 반려·재생성 11장. 에셋 `LOWVISIONPC1_01~16 / PC2_01~12 / PC3_01~11`.

### 8. 스킬·운영 갱신

- `blog-publish`: 낡은 "홈 3섹션 IA·`#기획`" 규칙 폐기 → 재생목록 8개 표 / **⑥-1 카테고리 동기화** 신설 / **⑥-2 방문자 통계** 신설 / **② 선정에 `ai-news-gate` 게이트** 물림 / 함정 노트 4건 추가
- `ghost-blogger`: Step 3 카테고리 표 + **Step 3-1 동기화** 신설
- 스케줄: Cowork → Claude Code 이관. `daily-news-blog-publish`, cron `0 10,23 * * *`, **하루 총 3건 상한**(회차당 아님). 23시 회차는 23:59 영상 작업 때문에 ComfyUI를 내리지 않도록 명시
- Outline 커리큘럼 문서 `b531f448`: 구 규격 6곳 정정 표기, §8 진행 현황 갱신, 중복 §15 → `15-B` 재번호

## 진행 중 / 미완료 작업

없음. 착수한 작업은 모두 마무리했다.

## 주요 결정사항 / 규칙

1. **카테고리 정본은 유튜브 재생목록이다.** 블로그 카테고리를 따로 만들지 않는다.
2. **카테고리 태그의 slug를 바꾸거나 지우지 않는다.** 홈 섹션이 그 slug로 돌아간다.
3. **태그 정리 전에 홈을 먼저 연다.** 태그 목록만 보고 판단하면 깨진다.
4. **이미지 생성 전 VRAM 15GB 이상 확보** — ComfyUI 프로세스 재기동으로만 가능.
5. **`svil_generate_image`는 한 번에 한 장씩.** 병렬 호출은 락 좀비를 만든다.
6. 발행 직후 **`npm run tags:sync`** 생략 금지.
7. 뉴스 주제는 **`ai-news-gate`** 두 축 통과분만. 접근성은 필터가 아니다.

## 참고 정보

- 완료보고서: `docs/reports/report_20260807_블로그카테고리통일_통계신설_저시력3부작_ClaudeCode.md` (커밋 `484ff98`)
- ghost 저장소 커밋: `7f3609b`(테마 8섹션) `bcb57b1`(Umami) `1c50471`(비밀번호) `4be45e8`(삭제분 보관)
- Outline 「일일 현황」 `e592fd0d-217c-4612-841c-a232ae3b19bc` — 2026-08-06/07 항목
- 이미지 규칙 정본: `C:\youtube\docs\가이드_20260731_블로그이미지규칙_ClaudeCode.md`
- 블로그 현황: 148편(공개 130 / draft 17 / #hidden 1), 플레이스홀더 0, 홈 9섹션 정상

## 다음 세션 시작 시 할 일

1. ⚠️ **Cowork 스케줄 생존 확인** — 8/6 17:19·17:23 발행분이 새 스케줄(10:00·23:00)과 안 맞는다. 살아 있으면 **중복 발행 위험**. Cowork에서 삭제됐는지 확인할 것.
2. ⚠️ **저시력 1편 추정값 검증** — 「제 화면은 지금 이렇게 맞춰져 있습니다」의 마우스 포인터 형광 연두 / 그래픽카드 밝기 −15%·대비 +15% / 알림 30초, 2편 매크로패드 배치는 InBlue 실제 설정이 아니라 추정·예시다. 확인 후 수정 필요.
3. **`redirects.json` 업로드** — 구 태그 URL 5개가 404. Admin API 통합 토큰은 redirects 엔드포인트 403이라 Ghost 관리자에서 직접 업로드해야 한다. 파일은 이미 InBlue에게 전달됨.
4. **`daily-news-blog-publish` 첫 실행을 "지금 실행"으로** — 도구 승인을 저장해야 자동 실행이 권한 프롬프트에서 안 멈춘다.
5. **ghost 저장소 `feat/home-sections-youtube-playlists` 병합** — PR 대기 중.
6. **qwen3tts 정지 상태** — VRAM 확보하려고 내렸고 다시 안 올렸다. TTS 필요하면 기동.
7. **23:00 블로그 회차와 23:59 영상 작업 간격이 좁다.** 영상 작업을 새벽으로 미루는 편이 근본적.
8. **영상화 착수 가능** — 저시력 3부작 39장 + 소버린/TXTDrop 27장이 에셋으로 등록돼 있다.
