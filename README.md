# Claude Code 사용 팁
- 이 내용은 Claude Code의 창시자 Boris Cherny가 제안하는 Claude Code의 사용팁입니다.
- 원문 : https://nitter.net/bcherny/status/2007179836704600237


# Claude Code 창시자 Boris Cherny의 13가지 사용 팁

저는 Boris이고 Claude Code를 만들었습니다. 많은 분들이 제가 Claude Code를 어떻게 사용하는지 물어보셔서, 제 설정을 조금 공유하고 싶었습니다.

제 설정은 놀라울 정도로 평범(vanilla)할 수 있습니다! Claude Code는 기본 상태에서도 훌륭하게 작동하기 때문에, 저는 개인적으로 많이 커스터마이징하지 않습니다. Claude Code를 사용하는 올바른 방법은 하나가 아닙니다. 우리는 의도적으로 여러분이 원하는 대로 사용하고, 커스터마이징하고, 해킹할 수 있도록 만들었습니다. Claude Code 팀의 각 구성원도 모두 매우 다르게 사용합니다.

---

## 1. 터미널에서 5개의 Claude를 병렬로 실행

터미널에서 5개의 Claude를 병렬로 실행합니다. 탭에 1-5로 번호를 매기고, Claude가 입력이 필요할 때 시스템 알림을 사용합니다.

🔗 [터미널 알림 설정 문서](https://code.claude.com/docs/en/terminal-config#iterm-2-system-notifications)

![터미널 병렬 실행](https://pbs.twimg.com/media/G9rtc4EasAELEzh.jpg)

---

## 2. 웹과 로컬을 함께 활용

[claude.ai/code](http://claude.ai/code)에서 5-10개의 Claude를 로컬 Claude와 병렬로 실행합니다. 터미널에서 코딩하면서 자주 로컬 세션을 웹으로 넘기거나(`&` 사용), Chrome에서 수동으로 세션을 시작하고, 때로는 `--teleport`로 왔다 갔다 합니다.

또한 매일 아침과 하루 종일 휴대폰(Claude iOS 앱)에서 몇 개의 세션을 시작하고, 나중에 확인합니다.

![웹과 로컬 병렬 사용](https://pbs.twimg.com/media/G9reZjqaYAA9btU.jpg)

---

## 3. 모든 작업에 Opus 4.5 + Thinking 사용

모든 작업에 Opus 4.5와 thinking을 사용합니다. 제가 사용해본 최고의 코딩 모델이며, Sonnet보다 크고 느리지만, 조종(steer)을 덜 해도 되고 도구 사용에 더 뛰어나기 때문에 결국 더 작은 모델을 사용하는 것보다 거의 항상 더 빠릅니다.

---

## 4. 팀 공유 CLAUDE.md 관리

팀이 Claude Code 저장소에 대해 하나의 `CLAUDE.md`를 공유합니다. git에 체크인하고, 팀 전체가 매주 여러 번 기여합니다. Claude가 무언가를 잘못하는 것을 볼 때마다 `CLAUDE.md`에 추가해서, Claude가 다음에는 그렇게 하지 않도록 합니다.

다른 팀들은 자체 `CLAUDE.md`를 유지합니다. 각 팀이 자신의 것을 최신 상태로 유지하는 것이 해당 팀의 책임입니다.

![CLAUDE.md 예시](https://pbs.twimg.com/media/G9rfKYRbkAA6Q3w.jpg)

---

## 5. 코드 리뷰에서 @.claude 태그 활용

코드 리뷰 중에 동료의 PR에 `@.claude`를 태그하여 PR의 일부로 `CLAUDE.md`에 무언가를 추가하는 경우가 많습니다. 이를 위해 Claude Code GitHub Action(`/install-github-action`)을 사용합니다. 이것은 @danshipper의 "Compounding Engineering" `CLAUDE.md`에 대한 우리 버전입니다.

![GitHub Action 사용](https://pbs.twimg.com/media/G9rhsVFasAIUCYj.jpg)

---

## 6. Plan 모드로 세션 시작

대부분의 세션은 **Plan 모드**(`Shift+Tab` 두 번)로 시작합니다. 목표가 Pull Request를 작성하는 것이라면, Plan 모드를 사용하고 Claude의 계획이 마음에 들 때까지 왔다 갔다 합니다. 거기서부터 **auto-accept edits 모드**로 전환하면 Claude가 보통 한 번에 완성합니다.

**좋은 계획이 정말 중요합니다!**

![Plan 모드](https://pbs.twimg.com/media/G9rjZcwasAQpPN6.png)

---

## 7. 반복 작업을 위한 Slash Commands 활용

매일 여러 번 하게 되는 모든 "내부 루프" 워크플로우에 **slash commands**를 사용합니다. 이는 반복적인 프롬프팅을 줄여주고, Claude도 이러한 워크플로우를 사용할 수 있게 합니다. 명령어는 git에 체크인되고 `.claude/commands/`에 있습니다.

예를 들어, Claude와 저는 `/commit-push-pr` slash command를 매일 수십 번 사용합니다. 이 명령어는 인라인 bash를 사용하여 git status와 몇 가지 다른 정보를 미리 계산하여 명령어가 빠르게 실행되고 모델과의 왕복을 피합니다.

🔗 [Slash Commands 문서](https://code.claude.com/docs/en/slash-commands#bash-command-execution)

![Slash Commands 예시](https://pbs.twimg.com/media/G9rj3eFasAEK_8J.jpg)

---

## 8. 서브에이전트(Subagents) 정기적 활용

몇 가지 **서브에이전트**를 정기적으로 사용합니다:
- **code-simplifier**: Claude가 작업을 마친 후 코드를 단순화
- **verify-app**: Claude Code를 처음부터 끝까지 테스트하기 위한 상세 지침
- 기타 등등

slash commands와 마찬가지로, 서브에이전트는 대부분의 PR에 대해 수행하는 가장 일반적인 워크플로우를 자동화하는 것으로 생각합니다.

🔗 [Subagents 문서](https://code.claude.com/docs/en/sub-agents)

![Subagents 예시](https://pbs.twimg.com/media/G9rnUzEasAElFcN.png)

---

## 9. PostToolUse Hook으로 코드 포맷팅

**PostToolUse hook**을 사용하여 Claude의 코드를 포맷팅합니다. Claude는 보통 기본적으로 잘 포맷된 코드를 생성하고, hook이 나중에 CI에서 포맷팅 오류를 피하기 위해 나머지 10%를 처리합니다.

![PostToolUse Hook](https://pbs.twimg.com/media/G9rrnTxasAAMoZ_.jpg)

---

## 10. 권한 설정 최적화 (/permissions 활용)

`--dangerously-skip-permissions`를 사용하지 않습니다. 대신 `/permissions`를 사용하여 환경에서 안전하다고 알고 있는 일반적인 bash 명령어를 미리 허용하여 불필요한 권한 프롬프트를 피합니다.

이들 대부분은 `.claude/settings.json`에 체크인되어 팀과 공유됩니다.

![권한 설정](https://pbs.twimg.com/media/G9rlDa-asAAXlHx.jpg)

---

## 11. Claude Code가 모든 도구를 대신 사용

Claude Code가 저를 위해 모든 도구를 사용합니다:
- **Slack** 검색 및 게시 (MCP 서버를 통해)
- 분석 질문에 답하기 위한 **BigQuery** 쿼리 실행 (bq CLI 사용)
- **Sentry**에서 오류 로그 가져오기
- 기타 등등

Slack MCP 구성은 `.mcp.json`에 체크인되어 팀과 공유됩니다.

![MCP 도구 활용](https://pbs.twimg.com/media/G9rl_pQb0AAILz8.jpg)

---

## 12. 장시간 실행 작업 관리

매우 오래 실행되는 작업의 경우, 다음 중 하나를 수행합니다:
- (a) 완료되면 백그라운드 에이전트로 작업을 확인하도록 Claude에 프롬프트
- (b) 에이전트 **Stop hook**을 사용하여 더 결정론적으로 수행
- (c) **ralph-wiggum 플러그인** 사용 (원래 @GeoffreyHuntley가 고안)

또한 세션에서 권한 프롬프트를 피하기 위해 샌드박스에서 `--permission-mode=dontAsk` 또는 `--dangerously-skip-permissions`를 사용하여 Claude가 저를 기다리지 않고 작업할 수 있게 합니다.

🔗 [ralph-wiggum 플러그인](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)  
🔗 [Hooks 가이드](https://code.claude.com/docs/en/hooks-guide)

![장시간 작업 관리](https://pbs.twimg.com/media/G9ro4W5bEAAQ3ug.jpg)

---

## 13. 🌟 가장 중요한 팁: Claude에게 검증 방법 제공

**마지막 팁이자 가장 중요한 것**: Claude Code에서 훌륭한 결과를 얻기 위해 가장 중요한 것은 **Claude에게 자신의 작업을 검증할 방법을 제공하는 것**입니다. Claude가 그 피드백 루프를 가지면, 최종 결과의 품질이 **2-3배** 향상됩니다.

Claude는 **Claude Chrome extension**을 사용하여 [claude.ai/code](http://claude.ai/code)에 랜딩하는 모든 변경 사항을 테스트합니다. 브라우저를 열고, UI를 테스트하고, 코드가 작동하고 UX가 좋을 때까지 반복합니다.

검증은 각 도메인마다 다르게 보입니다:
- bash 명령어 실행처럼 간단할 수 있음
- 테스트 스위트 실행
- 브라우저나 폰 시뮬레이터에서 앱 테스트

**이것을 견고하게 만드는 데 투자하세요.**

🔗 [Claude Chrome Extension 문서](https://code.claude.com/docs/en/chrome)

---

## 마무리

이 글이 도움이 되었기를 바랍니다! Claude Code를 사용하는 여러분만의 팁은 무엇인가요? 다음에 어떤 내용을 듣고 싶으신가요?

---

### 참고 링크 모음

| 주제 | 링크 |
|------|------|
| 터미널 알림 설정 | [Terminal Config](https://code.claude.com/docs/en/terminal-config#iterm-2-system-notifications) |
| Slash Commands | [Slash Commands Docs](https://code.claude.com/docs/en/slash-commands) |
| Subagents | [Subagents Docs](https://code.claude.com/docs/en/sub-agents) |
| Hooks 가이드 | [Hooks Guide](https://code.claude.com/docs/en/hooks-guide) |
| Chrome Extension | [Chrome Extension Docs](https://code.claude.com/docs/en/chrome) |
| ralph-wiggum 플러그인 | [GitHub](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum) |
