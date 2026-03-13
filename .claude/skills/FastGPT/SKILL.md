# FastGPT Development Patterns

> Auto-generated skill from repository analysis

## Overview

FastGPT is a TypeScript-based AI application platform that provides workflow automation, API endpoints, and multi-language support. The codebase follows modern full-stack patterns with emphasis on internationalization, Docker deployment, security, and modular architecture. The project uses a monorepo structure with packages for services, web components, and global utilities.

## Coding Conventions

### File Naming
- **Pattern:** camelCase for all files
- **Examples:** `workflowDispatch.ts`, `systemUtils.ts`, `apiController.ts`

### Import Style
- **Pattern:** Alias imports preferred
- **Examples:**
```typescript
import { WorkflowNodeType } from '@/types/workflow'
import { getSystemConfig } from '@/utils/system'
import type { ApiResponse } from '@fastgpt/global/api'
```

### Export Style
- **Pattern:** Mixed exports (both named and default)
- **Examples:**
```typescript
// Named exports
export const validateRequest = () => {}
export type RequestConfig = {}

// Default exports for main components
export default function WorkflowDispatcher() {}
```

### Commit Conventions
- **Prefixes:** `fix:`, `feat:`, `chore:`, `perf:`, `docs:`
- **Length:** Average 38 characters
- **Examples:**
```
feat: add workflow tool enhancement
fix: security vulnerability in utils
docs: update v4.14 deployment guide
```

## Workflows

### Documentation Update
**Trigger:** When releasing a new version or updating documentation
**Command:** `/update-docs`

1. Update version documentation files in `document/content/docs/upgrading/4-14/*.mdx`
2. Modify Docker Compose files with new image tags across deployment configs
3. Update `document/data/doc-last-modified.json` with timestamp information
4. Sync documentation changes across CN and global deployment configurations

```typescript
// Example doc update structure
const docUpdate = {
  version: "4.14.x",
  lastModified: Date.now(),
  deploymentConfigs: ["cn", "global"]
}
```

### Docker Compose Sync
**Trigger:** When updating deployment configurations or Docker image versions
**Command:** `/sync-docker`

1. Update main Docker Compose files in `deploy/docker/cn/` and `deploy/docker/global/`
2. Sync identical changes to `document/public/deploy/docker/` mirror locations
3. Update both development and production variants (docker-compose.yml, docker-compose.dev.yml)
4. Verify image tags and environment variables consistency

```yaml
# Example docker-compose structure
services:
  fastgpt:
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/fastgpt:v4.14.x
    environment:
      - DATABASE_URL=${DATABASE_URL}
```

### Security Fix Implementation
**Trigger:** When addressing security vulnerabilities or hardening features
**Command:** `/security-fix`

1. Modify core security functions in `packages/service/common/system/utils.ts`
2. Update HTTP security middleware in `packages/service/core/app/http.ts`
3. Add comprehensive test coverage in `test/cases/service/common/system/utils.test.ts`
4. Create upgrade documentation in `document/content/docs/self-host/upgrading/4-14/*.mdx`

```typescript
// Example security utility
export const sanitizeInput = (input: string): string => {
  return input.replace(/[<>\"']/g, '');
};

export const validateApiKey = (key: string): boolean => {
  return /^[a-zA-Z0-9-_]{32,}$/.test(key);
};
```

### Translation and i18n
**Trigger:** When adding new features or updating UI text that needs translation
**Command:** `/update-i18n`

1. Update source language files in `packages/web/i18n/zh/common.json` and `workflow.json`
2. Translate content to English and Traditional Chinese variants
3. Update documentation translations with `.en.mdx` files
4. Sync translation keys across all locale files and meta.json configurations

```json
// Example i18n structure
{
  "common": {
    "save": "保存",
    "cancel": "取消",
    "confirm": "确认"
  },
  "workflow": {
    "create": "创建工作流",
    "dispatch": "执行工作流"
  }
}
```

### API Endpoint Development
**Trigger:** When adding new API functionality or endpoints
**Command:** `/new-api`

1. Create API route handler in `projects/app/src/pages/api/core/` with appropriate subdirectory
2. Define TypeScript request/response interfaces in `packages/global/*/api.ts`
3. Implement authentication and permission validation middleware
4. Update service controller functions in `packages/service/*/controller.ts`

```typescript
// Example API endpoint structure
import type { NextApiRequest, NextApiResponse } from 'next';
import { authUser } from '@/middleware/auth';

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  try {
    const user = await authUser(req);
    // API logic here
    res.json({ success: true, data: result });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
}
```

### Workflow Tool Enhancement
**Trigger:** When adding new workflow capabilities or tool integrations
**Command:** `/enhance-workflow`

1. Update workflow dispatch tools in `packages/service/core/workflow/dispatch/tools/`
2. Modify core workflow types in `packages/global/core/workflow/type/`
3. Add tool-specific constants to `packages/global/core/workflow/constants.ts`
4. Implement tool execution logic and error handling

```typescript
// Example workflow tool structure
export const WorkflowNodeType = {
  HTTP_REQUEST: 'httpRequest',
  AI_CHAT: 'aiChat',
  CODE_RUN: 'codeRun'
} as const;

export interface WorkflowTool {
  nodeType: keyof typeof WorkflowNodeType;
  execute: (params: any) => Promise<any>;
}
```

### Dependency Updates
**Trigger:** When automated dependency scanning detects outdated packages
**Command:** `/update-deps`

1. Review Dependabot-created PRs for security and compatibility
2. Update package.json files across the monorepo structure
3. Regenerate lock files (pnpm-lock.yaml, package-lock.json)
4. Run tests to ensure compatibility with updated dependencies

## Testing Patterns

### Test Framework
- **Framework:** Vitest
- **Pattern:** `*.test.ts` files co-located with source code

### Test Structure
```typescript
import { describe, it, expect } from 'vitest';
import { sanitizeInput, validateApiKey } from '@/utils/security';

describe('Security Utils', () => {
  it('should sanitize malicious input', () => {
    const maliciousInput = '<script>alert("xss")</script>';
    const sanitized = sanitizeInput(maliciousInput);
    expect(sanitized).not.toContain('<script>');
  });

  it('should validate API key format', () => {
    expect(validateApiKey('valid-key-123_ABC')).toBe(true);
    expect(validateApiKey('invalid key')).toBe(false);
  });
});
```

## Commands

| Command | Purpose |
|---------|---------|
| `/update-docs` | Update version documentation and deployment guides |
| `/sync-docker` | Synchronize Docker Compose configurations across environments |
| `/security-fix` | Implement security fixes with testing and documentation |
| `/update-i18n` | Update internationalization files and translations |
| `/update-deps` | Update package dependencies and lock files |
| `/new-api` | Create new API endpoints with proper typing and auth |
| `/enhance-workflow` | Add new workflow tools and capabilities |