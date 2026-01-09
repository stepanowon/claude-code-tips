# 훅을 이용한 알림 기능

## Claude Code 병렬 사용시 알림 기능 추가
---
### Mac
- claude code 설정 파일(~/.claude/settings.json)에 다음 내용의 설정 추가하여 알림 소리 나도록
```
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Tink.aiff"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Funk.aiff"
          }
        ]
      }
    ]
  }
}
```
---
### Windows
- 윈도우의 경우 소리만 나게 할 것이 아니라 윈도우 알림에 포함시키도록 하고 윈도우 알림음을 설정할 수 있음
- 별도의 script 파일 작성. 반드시 저장할 때 UTF-8(BOM) 형식으로 저장할 것
```
# ~/.claude/scripts/toast.ps1 파일 생성
param(
  [ValidateSet("Notification","Stop")]
  [string]$Event = "Notification"
)

$LogDir  = "C:\temp\logs"
$LogFile = Join-Path $LogDir "claude-toast.log"

function Write-Log {
  param(
    [ValidateSet("INFO","WARN","ERROR")]
    [string]$Level,
    [string]$Text
  )
  if (!(Test-Path $LogDir)) { New-Item -ItemType Directory -Path $LogDir -Force | Out-Null }
  $ts = (Get-Date).ToString("yyyy-MM-dd HH:mm:ss.fff")
  Add-Content -Path $LogFile -Encoding UTF8 -Value "[$ts][$Level] $Text"
}

try {
  # 이벤트별로 환경변수에서 메시지/사유를 가져오되,
  # 사용자 요구대로 "title 한 줄"에 전부 합쳐 넣음
  $title = ""

  if ($Event -eq "Notification") {
    $note = if ($env:CLAUDE_NOTIFICATION) { $env:CLAUDE_NOTIFICATION } else { "Action required" }
    # 모든 정보를 title 한 줄로
    $title = "Claude Code 알림 | event=Notification | note=$note"
  }
  else {
    $reason = if ($env:CLAUDE_STOP_REASON) { $env:CLAUDE_STOP_REASON } else { "completed" }
    $title = "Claude Code 작업 완료 | event=Stop | reason=$reason"
  }

  # 로그도 같은 title 문자열만 남김 (구분필드 없음)
  Write-Log INFO $title

  [Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] | Out-Null

  $xml = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent(
    [Windows.UI.Notifications.ToastTemplateType]::ToastText02
  )

  # NodeList를 배열로 고정 후 InnerText로 안전하게 세팅
  $texts = @($xml.GetElementsByTagName("text"))
  if ($texts.Count -lt 2) { throw "Toast template text nodes not found (count=$($texts.Count))." }

  $texts[0].InnerText = $title
  $texts[1].InnerText = "Claude Code"

  [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier("Claude Code").Show(
    [Windows.UI.Notifications.ToastNotification]::new($xml)
  )
}
catch {
  Write-Log ERROR ("Exception: " + $_.Exception.Message)
  Write-Log ERROR ("ScriptStackTrace: " + $_.ScriptStackTrace)
  Write-Log ERROR ("ErrorRecord: " + ($_ | Out-String))
}
```
- claude code 설정 파일(~/.claude/settings.json)에 다음 내용의 설정 추가하여 윈도우 알림 기능 추가
```
{
  ......
  "hooks": {
    "Notification": [
      {
        "matcher": "*",
        "hooks": [
{
  "type": "command",
  "command": "powershell.exe -NoProfile -ExecutionPolicy Bypass -File \"C:\\Users\\stepa\\.claude\\scripts\\toast.ps1\" -Event Notification"
}

        ]
      }
    ],
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
{
  "type": "command",
  "command": "powershell.exe -NoProfile -ExecutionPolicy Bypass -File \"C:\\Users\\stepa\\.claude\\scripts\\toast.ps1\" -Event Stop"
}
        ]
      }
    ]
  }
}
```

- Claude Code 훅은 윈도우 알림 기능만 설정하므로 효과음을 나게 하려면 다음 설정 추가
  * 윈도우 11의 설정 - 시스템 - 소리 로 이동
  * 고급 - 더많은 소리 설정 으로 이동
  * 소리 탭으로 이동하여 '프로그램 이벤트'에서 '알림'을 찾아 소리를 지정함. 
