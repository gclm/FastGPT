```markdown
# FastGPT Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill provides a comprehensive guide to contributing to the FastGPT codebase, a TypeScript project focused on AI-powered features. It covers coding conventions, commit practices, and detailed, repeatable workflows for development, deployment, testing, and documentation. Whether you're adding features, updating deployment files, or managing assets, this guide ensures consistency and quality across contributions.

## Coding Conventions

- **Language:** TypeScript
- **Framework:** None detected (custom architecture)
- **File Naming:** Use `camelCase` for file and directory names.
  - Example: `userProfile.ts`, `apiHandler.ts`
- **Import Style:** Use aliases for imports.
  - Example:
    ```typescript
    import { fetchData } from '@/utils/api';
    ```
- **Export Style:** Mixed (both named and default exports are used).
  - Named export:
    ```typescript
    export function processInput(input: string) { ... }
    ```
  - Default export:
    ```typescript
    export default MyComponent;
    ```
- **Commit Patterns:**
  - Prefixes: `fix`, `chore`, `feat`, `perf`, `docs`
  - Example: `feat: add support for new database`
  - Keep commit messages concise (~36 characters on average).

## Workflows

### Update Docker Compose and Docs
**Trigger:** When updating deployment configurations, supporting new databases, or changing deployment instructions.  
**Command:** `/sync-docker-compose-docs`

1. Edit Docker Compose YAML files under:
   - `deploy/docker/cn/`
   - `deploy/docker/global/`
   - `deploy/templates/`
2. Update corresponding copies in `document/public/deploy/docker/`.
3. Optionally update `deploy/args.json` and any `deploy/dev/docker-compose*.yml` files.
4. Update or create documentation in:
   - `document/content/docs/self-host/upgrading/*`
   - `document/data/doc-last-modified.json`

**Example:**
```bash
# Edit a deployment file
vim deploy/docker/global/docker-compose.mysql.yml

# Sync to public docs
cp deploy/docker/global/docker-compose.mysql.yml document/public/deploy/docker/global/
```

---

### Feature Development with Tests and Docs
**Trigger:** When adding a significant new capability or refactoring a major subsystem.  
**Command:** `/feature-dev`

1. Implement the feature in core packages:
   - `packages/service/core/...`
   - `packages/global/core/...`
2. Update or add related frontend components:
   - `projects/app/src/components/`
   - `projects/app/src/pageComponents/`
3. Update or add API endpoints:
   - `projects/app/src/pages/api/`
4. Add or update tests in `test/cases/`.
5. Update or add documentation:
   - `document/content/docs/*`
   - `document/data/doc-last-modified.json`
6. Update i18n files if the UI is affected.

**Example:**
```typescript
// packages/service/core/myFeature.ts
export function newFeature() { ... }

// test/cases/myFeature.test.ts
import { newFeature } from '@/service/core/myFeature';
import { describe, it, expect } from 'vitest';

describe('newFeature', () => {
  it('should work as expected', () => {
    expect(newFeature()).toBeTruthy();
  });
});
```

---

### Update i18n and Icon Assets
**Trigger:** When updating UI translations or iconography, or cleaning up unused assets.  
**Command:** `/update-i18n-icons`

1. Edit or clean up translation files in `packages/web/i18n/` for all supported languages.
2. Edit or clean up SVG icon files in `packages/web/components/common/Icon/icons/`.
3. Update icon constants in `packages/web/components/common/Icon/constants.ts`.
4. Optionally, add scripts or documentation for asset management.

**Example:**
```json
// packages/web/i18n/en.json
{
  "welcome": "Welcome to FastGPT!"
}
```
```typescript
// packages/web/components/common/Icon/constants.ts
export const ICONS = {
  user: 'user.svg',
  chat: 'chat.svg',
};
```

---

### Version Bump and Release Prep
**Trigger:** When preparing for a new release or patch.  
**Command:** `/bump-version`

1. Update version numbers in `package.json` files across all projects.
2. Update `deploy/args.json` and Docker Compose files if needed.
3. Update `document/data/doc-last-modified.json` and upgrade notes in `document/content/docs/self-host/upgrading/*`.

**Example:**
```json
// projects/app/package.json
{
  "version": "1.2.3"
}
```

---

### Add or Update Backend Env Vars
**Trigger:** When new configuration options are needed for backend features.  
**Command:** `/update-env-vars`

1. Edit `packages/service/env.ts` or similar environment files.
2. Update `.env.template` files in relevant projects.
3. Update documentation if the change is public-facing.
4. Add or update tests if logic depends on new environment variables.

**Example:**
```typescript
// packages/service/env.ts
export const NEW_FEATURE_ENABLED = process.env.NEW_FEATURE_ENABLED === 'true';
```
```env
# projects/app/.env.template
NEW_FEATURE_ENABLED=true
```

## Testing Patterns

- **Framework:** [vitest](https://vitest.dev/)
- **Test File Pattern:** `*.test.ts` (e.g., `userService.test.ts`)
- **Test Location:** `test/cases/`
- **Example:**
  ```typescript
  import { describe, it, expect } from 'vitest';
  import { myFunction } from '@/service/core/myModule';

  describe('myFunction', () => {
    it('returns correct value', () => {
      expect(myFunction(2)).toBe(4);
    });
  });
  ```

## Commands

| Command                   | Purpose                                                        |
|---------------------------|----------------------------------------------------------------|
| /sync-docker-compose-docs | Synchronize Docker Compose files and update deployment docs     |
| /feature-dev              | Start a new feature with implementation, tests, and documentation |
| /update-i18n-icons        | Update translation files and icon assets                       |
| /bump-version             | Prepare for a new release by bumping version numbers           |
| /update-env-vars          | Add or update backend environment variables                    |
```
