# 핸드오프_20260803_0910_블로그파이프라인_C-Projects-blogger_images-claude-worktrees-ai-news-ghost-post-4347cb_클로드강의38회이미지완성

## 대상
- 프로젝트: 블로그파이프라인 (blogger_images)
- 작업 폴더: `C:\Projects\blogger_images\.claude\worktrees\ai-news-ghost-post-4347cb`
- 세션 시각: 2026-08-03 09:10 (KST)

## 세션 요약

《클로드 기초 및 활용》 4~38회 Ghost 발행글의 `[이미지 추후 생성]` 플레이스홀더를 실제 이미지로 채우는 작업. 다른 세션과 병렬 진행이라 충돌을 피해 **38회부터 역순**으로 담당했다.

## 완료된 작업

### 이미지 채우기 — 전 회차 완료

**4~38회 전 35편, 플레이스홀더 잔여 0.**

| 구간 | 담당 |
|---|---|
| 4~12회, 14~25회 | 다른 세션 |
| 13회, 26~38회 (14편) | 본 세션 |

문장으로도 적는다. **본 세션은 13회와 26~38회 총 14편을 담당해 이미지 152장을 채웠고, 나머지 구간은 병렬로 돌던 다른 세션이 처리했다.**

본 세션이 갱신한 post id (역순):

| 회차 | post id |
|---|---|
| 38 | `6a6e800b4c2dac0001ae08d6` |
| 37 | `6a6e7f874c2dac0001ae08c9` |
| 36 | `6a6e7f124c2dac0001ae08bc` |
| 35 | `6a6e7e7b4c2dac0001ae08af` |
| 34 | `6a6e7e124c2dac0001ae08a2` |
| 33 | `6a6e7da54c2dac0001ae0895` |
| 32 | `6a6e7d3c4c2dac0001ae0888` |
| 31 | `6a6e7ccb4c2dac0001ae087b` |
| 30 | `6a6e7c644c2dac0001ae086e` |
| 29 | `6a6e7bf74c2dac0001ae0861` |
| 28 | `6a6e7b8b4c2dac0001ae0854` |
| 27 | `6a6e7b274c2dac0001ae0847` |
| 26 | `6a6e7abd4c2dac0001ae083a` |
| 13 | `6a6e741a4c2dac0001ae078f` |

### 자산 라이브러리 등록

`C:\youtube\asset\img\CLAUDE1{회차}_{KEYWORD}_01.png` 형식으로 전량 등록. 4~38회 전 회차 커버리지 확인 완료(회차당 10~22장).

### 중복 발행 2건 — 해소 확인

`ghost_list_posts`로 시리즈 전체 조회 결과 **총 38건, 1~38회 각 1건씩**. 커리큘럼 문서 §15의 "중복 2건 대기"는 낡은 기록이었고 이미 정리돼 있었다. 삭제 대상이 없어 아무것도 삭제하지 않았다.

- 4회 현존: `6a6e6f384c2dac0001ae072a` (이미지 16장)
- 5회 현존: `6a6e8b1b4c2dac0001ae08e5` (이미지 17장)

### 부수 수정

- **35회**: `</strong ,경우가 많습니다.` 형태의 태그 오류로 굵게 범위가 다음 문단까지 번지던 마크업 교정.

### 기록

- Outline 커리큘럼 문서(`b531f448-4026-42fb-94f3-f41cb1428b78`) §16, §16-1 추가
- Outline 「일일 현황」(`e592fd0d-217c-4612-841c-a232ae3b19bc`) 2026-08-02 항목 추가
- `docs/reports/report_20260802_클로드강의38회이미지완성_ClaudeCode.md`

## 진행 중 / 미완료 작업

없음. 이미지 채우기는 전 회차 완료.

## 주요 결정사항 / 규칙

### 새로 확인된 프롬프트 트랩 워드

기존 목록(`note` `plane` `seal` `transformer` `gauge` `constellation` `capsule` `silhouette` `chamber` `ribbon` `burning`)에 추가:

| 단어·표현 | 유발되는 것 | 치환 |
|---|---|---|
| `form`, `shape` | 사람 얼굴·인체 옆모습 | `monolith`, `cube`, `crystal shard` |
| `held side by side`, `holding` | 사람 손 | `standing side by side`, `resting on` |
| `casting mold` | 손바닥 형태 | `rectangular vessel` |
| `tag`, `label`, `sheet of paper` | 글자 각인 | `blank metal plate`, `folded metal sheet` |
| `exit route` | 비상구 픽토그램(사람) | `bright open rectangle of light` |
| `inert`, `nothing happening` | 앉아 있는 사람 | `dim static indicator` |

문장으로도 적는다. **`form`과 `shape`은 사람 얼굴·인체를 부르므로 구체 사물명으로 바꾼다. `holding`·`held`가 들어가면 손이 나온다. `mold`는 손바닥이 되고, `tag`·`label`·`paper`는 글자를 부른다. `exit`는 비상구 픽토그램(사람 형상)을 만든다.**

neg_prompt 추가 항목: `hand, finger, palm, head, profile, chair, seat, label, engraving, digit, numeral, question mark, waveform, sign, pictogram`

### 검수 결과 통계 (본 세션 152장 기준)

반려·재생성 13장. 사람 형상 6건, 텍스트·기호 렌더 5건, 대비 부족 1건, 개념 불일치 1건.

**컨택트시트(ffmpeg `tile=4x3`) 검수가 필수임이 재확인됐다.** 개별 확인이었으면 놓쳤을 건이 다수였다.

### ComfyUI

세션 시작 시 이미 가동 중이었고 다른 세션과 공유 상태라 **종료하지 않았다.** 메모리 규칙 "내가 올린 것만 내린다"에 따른 판단. 양쪽 세션이 모두 끝났다면 `svil_stop_service("comfyui")` 실행 가능.

## 참고 정보

- Ghost 이미지 base URL: `https://ghost-production-0ec2.up.railway.app/content/images/2026/08/`
- 이미지 파일 접두사: `cl{회차}_{번호}[b].png` (b = 재생성분)
- 작업 스크립트: 스크래치패드 `prep.py`(플레이스홀더 추출), `patch.py`(figure 치환) — 재사용 시 `export PYTHONIOENCODING=utf-8` 필수
- 이미지 규칙 정본: `가이드_20260731_블로그이미지규칙_ClaudeCode.md`
- 커리큘럼: Outline `b531f448-4026-42fb-94f3-f41cb1428b78` (§13 발행 규격, §16 이미지 완료 기록)

## 다음 세션 시작 시 할 일

1. **ComfyUI 종료 판단** — 다른 세션도 끝났으면 내린다.
2. **영상화 착수 가능** — 38회 전편에 이미지가 갖춰져 영상 파이프라인에서 그대로 재사용할 수 있다. 회차당 10~22장이 `C:\youtube\asset\img\CLAUDE1*`에 준비돼 있다.
3. **스크린샷 방침(커리큘럼 §10-2) 여전히 미결** — 이번 작업분도 스크린샷 0장. 조작 단계는 본문 텍스트로 완결시켜 학습 공백은 없다. UI 회차(6·8·9·33·34회)를 개선하려면 ⓐ+ⓒ 혼합(Claude Code가 캡처 목록 특정 → InBlue가 캡처) 확정이 필요하다.
4. **트랩 워드 목록을 가이드 문서에 반영** — 위 §주요 결정사항의 6건이 아직 `가이드_20260731_블로그이미지규칙` 문서에 미반영 상태다.
