# city-atelier-img

[city-atelier](https://github.com/polos0117/city-atelier)(도시 공방 — 실제 도시를 판타지 · 스팀펑크 · 사이버펑크 세 세계의 여성으로 의인화하는 도감)의 그림 저장소. 코드 저장소가 무거워지지 않게 그림은 여기 둔다 — Pages 로 `https://polos0117.github.io/city-atelier-img/img/<파일>` 에서 읽는다.

> **2026-11-28 부터 이 저장소의 `img/` 는 닫혔다**(약 1 GB — 사용자 「도시공방 이미지 저장소 바꿔줘. 2번으로」). 새 그림은 둘째 저장소 [city-atelier-img-2](https://github.com/polos0117/city-atelier-img-2) 의 `img/` 에 올린다. 여기는 이미 있는 이름의 교체만 받는다 — 새 이름이 오면 등록이 멈춘다. **기록(`records/`) · 잠금 · 바탕 그림 · 화풍 비교 · 지구본 모형은 계속 여기다.**

규칙은 코드 저장소 문서가 정본이다 — [IMAGE_RULES.md](https://github.com/polos0117/city-atelier/blob/main/IMAGE_RULES.md)(그림 만들기 · 파일 이름 · 기록 · 올리기) · [docs/IMAGE_AUTOMATION.md](https://github.com/polos0117/city-atelier/blob/main/docs/IMAGE_AUTOMATION.md)(GPT 세션 — 잠금 · 지시문 묶음 · 검수).

## 자리

```
img/<도시>_<세계>_<화풍>_f.webp           세계 그림. 1024×1536 (2:3). 세계는 fantasy · steampunk · cyberpunk
img/<도시>_<세계>_<화풍>_f_casualN.webp   일상컷 N — 세계마다 따로 1 부터(90 = 캐릭터만의 컷). 그 세계 그림을 참고 그림으로 그린다
img/thumb/<같은 이름>                     목록용(긴 변 512, webp 72). 워크플로가 만든다 — 손으로 넣지 않는다
records/<도시>/<세계>.<화풍>-ui.json       어떻게 만들었나 — 생성기의 「기록 내보내기」 JSON(+ generation · qa)
records/<도시>/<세계>-casualN.<화풍>-ui.json   일상컷의 기록 · <세계>-casual-plan.json 일상 계획
records/replacement.json                  바꾼 그림의 앞 sha256(webp · 원본 png) · 날짜 · 까닭
records/automation/claims/<도시>.json      GPT 세션 잠금(tools/image-claim.cjs 만 쓴다)
backgrounds/                              화면 바탕 그림
style-test/compare/<묶음>/                 화풍 비교 탭 묶음(tools/style-compare.py — 도감 그림 아님)
models/globe/<세계>.glb                    지구본 모형(첫 화면 three.js 길)
```

파일 이름이 곧 등록 정보다. `<도시>` 는 코드 저장소 `data/city-design.json` 의 도시 열쇠(`seoul` · `newyork` …, 밑줄 없음), `<화풍>` 은 `data/style.json` 의 화풍 열쇠(지금 기본 `kr_drawn_rim_dewy`). 성별 표시 `_f` 는 반드시 붙인다. 보기: `seoul_fantasy_kr_drawn_rim_dewy_f.webp` · `seoul_steampunk_kr_drawn_rim_dewy_f_casual1.webp`.
이름 꼴이 틀린 파일(세계 `mecha` · 없는 도시 · `_m` · 대문자 …)이 하나라도 있으면 등록기가 아무것도 적지 않고 멈춘다.

원본 PNG 는 올리지 않는다(`.gitignore`) — 기록에 sha256 만 적는다. 그림을 덮어쓰지 않는다 — 바꿔 달라고 한 그림만 바꾸고 앞 그림의 sha256 을 `records/replacement.json` 에 남긴다. 강제 push · 이력 재작성은 하지 않는다.

## 올리는 법

1. webp(품질 90, 1024×1536)를 `img/` 에, 기록을 `records/<도시>/` 에 넣고 일반 push 한다. 썸네일은 만들지 않는다.
2. `썸네일` 워크플로(`.github/workflows/thumbs.yml`)가 코드 저장소의 `tools/make-thumbs.py` 로 `img/thumb/` 를 만들어 되커밋하고, 코드 저장소에 `repository_dispatch`(`images-updated`)를 보내 `그림 등록` 을 깨운다 — 이 저장소의 Actions 비밀 `CODE_REPO_TOKEN`(Fine-grained token, 저장소 city-atelier 하나, Contents: Read and write)으로.
3. 코드 저장소의 `그림 등록` 이 이 저장소를 옆에 받아 `data/img.json` 에 적는다. 깨우기가 안 닿아도 한 시간마다(매시 41분) 스스로 돈다. 화면(도감 · 생성기)은 `data/img.json` 에 적힌 것만 그린다.

이 저장소가 900 MB 를 넘으면 `썸네일` 워크플로가 경고한다(Pages 사이트 한 개는 1 GB) — 그때는 둘째 그림 저장소를 연다(IMAGE_RULES.md 3절).
