# harness

스킬, 지켜야 할 규격, CI/CD를 한곳에서 관리하는 레포. 다른 6개 레포는 이 레포를 루트의 `skills/`로 참조한다(git submodule). 요구사항(FR/NFR)과 지표(E1~E10)는 [docs](https://github.com/sweg9/docs)의 `sim-demo/PRD.md`를 기준으로 한다.

## 구성

| 폴더 | 종류 | 하는 일 |
|---|---|---|
| `standards/` | 규격 | 모든 레포가 반드시 지켜야 하는 규칙. 공통(C-01~12), AI 출력(A-01~08), 로그(L-01~08), 데이터 모델, 레포별 의무 |
| `check-conformance/` | 스킬 | 코드, 프롬프트, 데이터가 규격을 지키는지 근거(파일과 줄)와 함께 점검 |
| `check-execution-log/` | 스킬 | 단계 기록 로그(JSON Lines)가 규격에 맞는지 검증 |
| `validate-dataset/` | 스킬 | 라벨 스키마, 개인정보, 학습·평가 분리를 점검 |
| `check-fr-coverage/` | 스킬 | FR-01~16마다 테스트가 있는지 확인 |
| `run-stage-eval/` | 스킬 | 로그에서 지표 E1~E10을 계산해 목표값과 비교 |
| `trace-failure/` | 스킬 | 실패한 실행을 처음 실패한 단계에 귀속하고 원인을 분류 |
| `prompt-regression/` | 스킬 | 프롬프트 새 버전을 고정 평가 세트로 이전 버전과 비교 |
| `build-sim-demo/` | 스킬 | sim-demo를 macOS, Windows로 빌드하고 zip으로 묶음 |
| `release-gate/` | 스킬 | 위 점검을 순서대로 돌려 go / no-go 판정 |

## 다른 레포에서 쓰기

각 레포 루트의 `skills/`가 이 레포의 특정 커밋을 가리키고, `.claude/skills`는 `../skills`로 연결된다. 그래서 Claude Code를 열면 스킬이 바로 보인다.

```bash
git clone --recurse-submodules <레포 주소>
git submodule update --init            # 이미 받은 레포에서
git submodule update --remote skills   # 스킬 최신 버전으로 올리기, 그 뒤 커밋
```

규격과 스킬은 이 레포에서만 고친다. 다른 레포의 `skills/` 안에서 직접 고치지 않는다.
