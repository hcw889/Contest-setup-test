
---
## ⏱️ 즉시 테스트: 2026-11-18(수) 16:00 ~ 22:00 차단, 22:00 자동 복구

16:00에 바로 설치하는 경우의 명령입니다. 새 관리자 PowerShell 창(`Win + X` → **터미널(관리자)**)에 통째로 붙여넣습니다. 날짜가 다르면 날짜 부분만 바꿉니다.

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
$env:CONTEST_EXAM_START = '2026-11-18T16:00:00'
$env:CONTEST_EXAM_END = '2026-11-18T22:00:00'
$env:CONTEST_RESTORE_AT = '2026-11-18T22:00:00'
irm https://raw.githubusercontent.com/naixt1478/ContestSetup/main/install-env.ps1 | iex
```

- 바로 설치하는 경우라 차단 유예(`CONTEST_DEFER_AI_BLOCK`)는 쓰지 않습니다. 설치는 몇 분 걸리고, AI 차단은 설치 마지막 단계에서 바로 걸립니다.
- 유예를 켠 채 시작 시각 직전에 설치하면, 설치 도중 몇 초 사이에 시작 시각이 지나는 경우 차단이 아예 걸리지 않는 틈이 코드에 있습니다. 유예를 쓸 때는 시작 시각보다 넉넉히 먼저 설치합니다.
- 22:00에 자동 복구가 끝나면 30초 뒤 PC가 꺼집니다. 끄지 않으려면 `irm` 줄 앞에 `$env:CONTEST_NO_RESTORE_SHUTDOWN = '1'`을 추가합니다. 카운트다운 중에는 `shutdown /a`로 종료만 취소할 수 있습니다.

설치 후 같은 관리자 창에서 확인합니다. `ContestSetupRestoreAndCleanup`이 22:00으로 나오면 정상입니다.

```powershell
Get-ScheduledTask -TaskName 'ContestSetup*' | Get-ScheduledTaskInfo | Select-Object TaskName, NextRunTime
```

테스트를 중간에 끝내려면 관리자 창에서 아래를 실행합니다. 바로 복구되고 PC는 꺼지지 않습니다.

```powershell
powershell -ExecutionPolicy Bypass -File C:\CPTools\restore-and-cleanup.ps1 -NoPause
```
