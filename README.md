# cicd-harness

CI/CD와 테스트·평가 하네스. 요구사항(FR/NFR)과 지표(E1~E10)는 [docs](https://github.com/sweg9/docs)의 `sim-demo/PRD.md`를 기준으로 한다.

## 스킬

`.claude/skills/`에 Claude Code 스킬을 둔다. 이 레포에서 Claude Code를 열면 자동으로 쓸 수 있다.

| 스킬 | 하는 일 |
|---|---|
| `build-sim-demo` | sim-demo Unity 프로젝트를 macOS, Windows로 빌드하고 zip으로 묶는다 |
| `run-stage-eval` | 실행 로그에서 지표 E1~E10을 계산해 목표값과 비교한다 |
| `trace-failure` | 실패한 실행을 처음 실패한 단계에 귀속하고 원인을 분류한다 |
| `check-fr-coverage` | FR-01~16마다 테스트가 있는지 확인하고 빈 곳을 보고한다 |
| `prompt-regression` | 프롬프트 새 버전을 고정 평가 세트로 이전 버전과 비교한다 |
