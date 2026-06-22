# 공통 — 취소 처리

> 이 문서는 `/sync --cancel` 또는 페이즈 실행 중 사용자가 "중단해줘" / "취소해줘" 라고 말한 경우에만 Read한다.
> 평상시 sync 흐름에서는 로드되지 않는다 (상시 컨텍스트 절약).

## 1. 상태 파일 확인
```bash
SYNC_BASE=$([ -d "$HOME/Documents/obsidian_vault" ] && echo "$HOME/Documents/obsidian_vault/Sync" || echo "$HOME/Downloads/Sync")
cat "$SYNC_BASE/$(basename {TO_PATH})/.sync_state.json" 2>/dev/null || echo "NO_STATE"
```

상태 파일이 없으면:
```
현재 진행 중인 sync 작업이 없어.
```

## 2. 변경된 파일 목록 출력

```bash
cd "{TO_PATH}" && git diff --name-only HEAD
```

아래 형식으로 출력:
```
🛑 sync 작업 취소

완료된 페이즈: {completed_phases 목록}
마지막 진행: Phase {next_phase}

변경된 파일 목록:
  - Assets/...파일명.cs
  - Assets/...파일명.prefab
  - ...

어떻게 처리할까?
  r → 변경사항 전체 되돌리기 (git checkout -- .)
  k → 변경사항 유지하고 상태 파일만 삭제
  a → 취소 중단 (sync 계속 진행)
```

## 3. 선택에 따른 처리

**r 선택 (변경사항 되돌리기):**
```bash
SYNC_BASE=$([ -d "$HOME/Documents/obsidian_vault" ] && echo "$HOME/Documents/obsidian_vault/Sync" || echo "$HOME/Downloads/Sync")
cd "{TO_PATH}" && git checkout -- .
rm "$SYNC_BASE/$(basename {TO_PATH})/.sync_state.json"
```
완료 후:
```
✅ 변경사항이 모두 되돌려졌어. sync 작업 전 상태로 복원됐어.
```

**k 선택 (상태 파일만 삭제):**
```bash
SYNC_BASE=$([ -d "$HOME/Documents/obsidian_vault" ] && echo "$HOME/Documents/obsidian_vault/Sync" || echo "$HOME/Downloads/Sync")
rm "$SYNC_BASE/$(basename {TO_PATH})/.sync_state.json"
```
완료 후:
```
✅ 상태 파일을 삭제했어. 변경된 파일은 그대로 유지돼.
나중에 /sync 로 새 sync 작업을 시작할 수 있어.
```

**a 선택 (취소 중단):**
```
계속 진행할게. Phase {next_phase}부터 이어서 진행하려면 "다음 페이즈" 라고 말해줘.
```
