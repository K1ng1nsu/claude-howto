<!-- i18n-source: 07-plugins/devops-automation/README.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../../resources/logos/claude-howto-logo.svg">
</picture>

# DevOps 자동화 플러그인

배포, 모니터링 및 인시던트 대응을 위한 완전한 DevOps 자동화.

## 기능

✅ 자동화된 배포
✅ 롤백 절차
✅ 시스템 상태 모니터링
✅ 인시던트 대응 워크플로우
✅ Kubernetes 통합

## 설치

```bash
/plugin install devops-automation
```

## 포함된 항목

### 슬래시 명령어
- `/deploy` - 프로덕션 또는 스테이징에 배포
- `/rollback` - 이전 버전으로 롤백
- `/status` - 시스템 상태 확인
- `/incident` - 프로덕션 인시던트 처리

### 서브에이전트
- `deployment-specialist` - 배포 작업
- `incident-commander` - 인시던트 조정
- `alert-analyzer` - 시스템 상태 분석

### MCP 서버
- Kubernetes 통합

### 스크립트
- `deploy.sh` - 배포 자동화
- `rollback.sh` - 롤백 자동화
- `health-check.sh` - 상태 확인 유틸리티

### 훅
- `pre-deploy.js` - 배포 전 검증
- `post-deploy.js` - 배포 후 작업

## 사용법

### 스테이징에 배포
```
/deploy staging
```

### 프로덕션에 배포
```
/deploy production
```

### 롤백
```
/rollback production
```

### 상태 확인
```
/status
```

### 인시던트 처리
```
/incident
```

## 요구사항

- Claude Code 1.0+
- Kubernetes CLI (kubectl)
- 클러스터 접근 설정됨

## 설정

Kubernetes 설정:
```bash
export KUBECONFIG=~/.kube/config
```

## 예제 워크플로우

```
User: /deploy production

Claude:
1. Runs pre-deploy hook (validates kubectl, cluster connection)
2. Delegates to deployment-specialist subagent
3. Runs deploy.sh script
4. Monitors deployment progress via Kubernetes MCP
5. Runs post-deploy hook (waits for pods, smoke tests)
6. Provides deployment summary

Result:
✅ Deployment complete
📦 Version: v2.1.0
🚀 Pods: 3/3 ready
⏱️  Time: 2m 34s
```

---

**마지막 업데이트**: 2026년 6월 2일
**Claude Code 버전**: 2.1.160
**출처**:
- https://code.claude.com/docs/en/plugins
- https://github.com/anthropics/claude-code/releases/tag/v2.1.131
- https://github.com/anthropics/claude-code/releases/tag/v2.1.138
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
