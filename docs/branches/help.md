# 도움말

> 이 문서는 사용자가 `/sync --help` 또는 사용법을 물어볼 때만 Read한다.
> 평상시 sync 흐름에서는 로드되지 않는다 (상시 컨텍스트 절약).

```
사용법:
  /sync
      순차 대화형 마법사 시작 (또는 이전 작업 이어서 진행)

  /sync --from <소스> --system <이름> --keys <키워드,...>
      TO는 현재 프로젝트 자동 감지, Phase는 대화로 선택

  /sync --from <소스> --to <경로> --system <이름> --keys <키워드,...> --phase <4|5a|5b|5c>
      모든 값 지정, 즉시 실행

  /sync --cancel
      진행 중인 sync 작업 취소. 변경사항 되돌리기 또는 상태 파일만 삭제 선택 가능.

  /sync --cleanup
      sync 완료 후 임시 worktree 정리.

--from 예시:
  로컬 경로:     --from /Volumes/repo/BunkerDefense
  remote 브랜치: --from origin/temp-bunker
  remote URL:   --from git@github.com:TeamSparta/BunkerDefense.git
  URL + 브랜치:  --from git@github.com:TeamSparta/BunkerDefense.git --branch develop

전체 예시:
  # 기본 (전체 분석 + 산출물 자동 저장)
  /sync --from origin/temp-bunker \
        --system BattlePass \
        --keys BattlePassManager,BattlePassTypes,BattlePass \
        --phase 5a
  # → 산출물: ~/Documents/obsidian_vault/Sync/2026-05-03_BattlePass/
  #           (obsidian 없으면 ~/Downloads/Sync/2026-05-03_BattlePass/)

  # 기존 분석 문서 활용 (Phase 1~3 스킵, 토큰 절약)
  /sync --from origin/temp-bunker \
        --system BattlePass \
        --plan ~/Documents/obsidian_vault/Sync/2026-05-03_BattlePass/BattlePass_SYNC_PLAN.md \
        --phase 4

  # 출력 경로 직접 지정 (폴백 없이 항상 이 경로 사용)
  /sync --from origin/temp-bunker \
        --system BattlePass \
        --keys BattlePassManager,BattlePassTypes \
        --output ~/Downloads/Sync/2026-05-03_BattlePass \
        --phase 5a

💡 컨텍스트 절약 팁:
   각 페이즈 완료 후 /clear 를 실행해도 설정이 보존돼.
   /sync 를 다시 실행하면 중단된 페이즈부터 이어서 진행해.
```
