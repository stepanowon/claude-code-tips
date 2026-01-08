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
- claude code 설정 파일(~/.claude/settings.json)에 다음 내용의 설정 추가하여 윈도우 알림 기능 추가
```
{
  "model": "sonnet",
"hooks": {
    "Notification": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -c \"[Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] | Out-Null; $xml = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent([Windows.UI.Notifications.ToastTemplateType]::ToastText02); $xml.GetElementsByTagName('text')[0].AppendChild($xml.CreateTextNode('Claude Code 알림')) | Out-Null; $msg = if ($env:CLAUDE_NOTIFICATION) { $env:CLAUDE_NOTIFICATION } else { 'Action required' }; $xml.GetElementsByTagName('text')[1].AppendChild($xml.CreateTextNode($msg)) | Out-Null; [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier('Claude Code').Show([Windows.UI.Notifications.ToastNotification]::new($xml))\""
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
            "command": "powershell -c \"[Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] | Out-Null; $xml = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent([Windows.UI.Notifications.ToastTemplateType]::ToastText02); $xml.GetElementsByTagName('text')[0].AppendChild($xml.CreateTextNode('Claude Code 작업 완료')) | Out-Null; $reason = if ($env:CLAUDE_STOP_REASON) { $env:CLAUDE_STOP_REASON } else { 'completed' }; $msg = \"Task $reason\"; $xml.GetElementsByTagName('text')[1].AppendChild($xml.CreateTextNode($msg)) | Out-Null; [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier('Claude Code').Show([Windows.UI.Notifications.ToastNotification]::new($xml))\""
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
