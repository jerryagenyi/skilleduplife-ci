# ✅ CI Setup Checklist (SkilledUp.Life)

> **Note:** This checklist covers both frontend (Vue) and backend (Laravel) CI setup with shared principles and separate implementation guides.

## 🧭 GUIDING PRINCIPLES

> **📖 Context:** For full platform, team dynamics, and engineering workflow context, see [SkilledUp.Life Context](./skilleduplife-context.md)

### 🎯 Why We Implement CI This Way

**Teaching vs Protecting Codebase:**
- **Protect** with linting/formatting + CI blocking
- **Teach** via transparent errors and README culture tips
- Even volunteers benefit from learning better coding habits through tooling

**Why CI doesn't auto-fix (and shouldn't):**
Auto-fixing in CI creates Git discrepancies. Developers should fix **locally**, learn from errors, and recommit. CI ensures accountability without intruding on authoring flow.

**Defining & Communicating Standards:**
1. **Define:** Codify in linting rules and formatting config
2. **Communicate:** Add usage notes in README and onboarding docs
3. **Enforce:** Via CI pipeline (already in place)
4. **Guide team:** Offer framework-specific rules that reflect clean architecture

---

## 📝 Test Coverage Tracker Setup

- [x] Create Google Sheet for test coverage tracker (Title: "Test Coverage Tracker – SkilledUp.Life Modules")
- [x] Test Coverage Tracker Link: [https://docs.google.com/spreadsheets/d/1q2xA4L0VjMm7GEi-NSbk-X55E4FXBHt9O2PLc0jlqhk/edit?gid=0#gid=0](https://docs.google.com/spreadsheets/d/1q2xA4L0VjMm7GEi-NSbk-X55E4FXBHt9O2PLc0jlqhk/edit?gid=0#gid=0)
- [ ] Add test coverage tracker link to frontend README
- [ ] Add test coverage tracker link to backend README

---

## 📚 README Documentation Setup

### Implementation Checklist
- [ ] Copy `ci-structure/README-frontend-draft.md` to `frontend/README.md`
- [ ] Copy `ci-structure/README-backend-draft.md` to `backend/README.md`
- [ ] Update Google Sheet links in both READMEs
- [ ] Commit and push README updates
- [ ] Verify README formatting in GitHub

📋 **README Drafts:** 
- Frontend: [`ci-structure/README-frontend-draft.md`](./README-frontend-draft.md)
- Backend: [`ci-structure/README-backend-draft.md`](./README-backend-draft.md)

---

## 📦 Shared Configuration Setup

> **📋 Detailed Guide:** [CI Shared Config Setup Checklist](./ci-shared/ci_shared_setup_checklist.md)

### 🎯 Purpose
This section covers setting up the shared configuration files that both frontend and backend repositories will use. These configs ensure consistency across all SkilledUp.Life projects.

### 📁 Shared Files Structure
```
.github/ci-shared/
├── eslint-config/
│   ├── eslint.vue.config.js        # Vue-specific linting
│   └── eslint.backend.config.js    # Backend-specific linting
├── prettier-config/
│   └── prettier.shared.config.js   # Shared formatting rules
└── test-config/
    ├── jest.vue.config.js          # Vue testing (future use)
    └── playwright.config.js        # E2E testing (Playwright)
```

### 🛠️ IDE Setup (VS Code/Cursor)
Copy `.vscode-settings.json` to `.vscode/settings.json` in your project for:
- Auto-formatting on save
- ESLint auto-fix on save
- Vue file support
- Prettier integration

### 🚀 Quick Shared Setup

1. **Create the shared folder structure:**
   ```bash
   mkdir -p .github/ci-shared/
   ```

2. **Copy shared config files:**
   ```bash
   # From this staging repo to your production repo
   cp ci-structure/ci-shared/prettier-config/prettier.base.config.js .github/ci-shared/prettier-config/prettier.frontend.config.js
   cp ci-structure/ci-shared/eslint-config/eslint.vue.config.js .github/ci-shared/eslint-config/      # Frontend only
   cp ci-structure/ci-shared/eslint-config/eslint.backend.config.js .github/ci-shared/eslint-config/  # Backend only
   cp ci-structure/ci-shared/test-config/jest.vue.config.js .github/ci-shared/test-config/        # Frontend only
   cp ci-structure/ci-shared/test-config/playwright.config.js .github/ci-shared/test-config/      # Frontend only
   ```

> **💡 Note:** If `.prettierrc.json` already exists in the root (like in the frontend repo), update it to extend the shared config instead of creating a new one. This maintains existing functionality while adding the shared structure.

3. **Create local config files that extend shared configs:**
   ```bash
   # .eslintrc.js (extends shared config)
   echo "module.exports = {
     ...require('./.github/ci-shared/eslint.vue.config'),
     // Add project-specific overrides here
   }" > .eslintrc.js
   
   # Update existing .prettierrc.json to extend shared config
   # (If .prettierrc.json already exists, update it instead of creating new)
   echo "module.exports = {
     ...require('./.github/ci-shared/prettier-config/prettier.frontend.config'),
     // Add project-specific overrides here
   }" > .prettierrc.json
   ```

### 📚 Shared Config Documentation
- **Shared Config README:** [ci-shared/README.md](./ci-shared/README.md)
- **Detailed Setup Guide:** [ci-shared/ci_shared_setup_checklist.md](./ci-shared/ci_shared_setup_checklist.md)

---

## 🚀 Frontend (Vue) Quick Setup

📋 **Detailed Frontend Setup:** [Frontend CI Setup Checklist](./ci_setup_checklist-fe.md)

### 📁 New Files & Folders to Create
- `.github/workflows/ci-frontend.yml` - GitHub Actions workflow
- `.github/ci-shared/prettier-config/prettier.frontend.config.js` - Frontend Prettier config
- `.github/ci-shared/eslint-config/eslint.vue.config.js` - Shared ESLint config
- `.github/ci-shared/test-config/jest.vue.config.js` - Shared Jest config (future use)
- `.eslintrc.js` - ESLint configuration (extends shared config)
- `.prettierrc.json` - Prettier configuration (extends shared config)
- `tests/unit/` - Unit test directory
- `tests/e2e/` - E2E test directory

### 🔧 Files to Modify
- `package.json` - Add lint/format scripts

**For developers who want to get CI running in 5 minutes:**

1. **Copy workflow template:**
   ```bash
   cp ci-structure/workflow-templates/ci-frontend.yml .github/workflows/
   ```

2. **Install ESLint:**
   ```bash
   npm install --save-dev eslint eslint-plugin-vue @vue/eslint-config-prettier prettier
   ```

3. **Copy config files:**
   ```bash
   cp ci-structure/ci-shared/eslint.vue.config.js .github/ci-shared/eslint.vue.config.js
   cp ci-structure/ci-shared/prettier-config/prettier.shared.config.js .github/ci-shared/prettier-config/prettier.shared.config.js
   cp ci-structure/ci-shared/jest.vue.config.js .github/ci-shared/jest.vue.config.js
   ```

4. **Add scripts to package.json:**
   ```json
   {
     "scripts": {
       "lint": "eslint src/ tests/ *.js --ext .js,.vue",
       "format": "prettier --check src/ tests/ *.js",
       "format:fix": "prettier --write src/ tests/ *.js"
     }
   }
   ```

5. **Create PR → see CI in action!**

📋 **Detailed Frontend Setup:** [Frontend CI Setup Checklist](./ci_setup_checklist-fe.md)

---

## 🚀 Backend (Laravel) Quick Setup

📋 **Detailed Backend Setup:** [Backend CI Setup Checklist](./ci_setup_checklist-be.md)

### 📁 New Files & Folders to Create
- `.github/workflows/ci-backend.yml` - GitHub Actions workflow
- `.github/ci-shared/prettier-config/prettier.backend.config.js` - Backend Prettier config
- `.github/ci-shared/eslint-config/eslint.backend.config.js` - Shared ESLint config
- `phpstan.neon` - PHPStan configuration
- `tests/Feature/` - Feature test directory
- `tests/Unit/` - Unit test directory

### 🔧 Files to Modify
- `composer.json` - Add lint/format/test scripts

**For developers who want to get CI running in 5 minutes:**

1. **Copy workflow template:**
   ```bash
   cp ci-structure/workflow-templates/ci-backend.yml .github/workflows/
   ```

2. **Install PHP dependencies:**
   ```bash
   composer install
   ```

3. **Setup Laravel environment:**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Run tests locally to verify:**
   ```bash
   php artisan test
   ```

5. **Create PR → see CI in action!**

📋 **Detailed Backend Setup:** [Backend CI Setup Checklist](./ci_setup_checklist-be.md)

---

## 🛡️ Rollback Strategy

**Before implementing CI changes:**
- [ ] Document current working state before CI changes
- [ ] Create a feature branch for CI implementation
- [ ] Test CI on the feature branch first
- [ ] Have a "disable CI" option for emergency fixes

**Branch-based testing approach:**
- Create a `ci-implementation` branch
- Test the entire workflow including PRs on this branch
- Only merge to main after thorough testing
- This allows full testing without affecting production development

---

## 🌍 Environment-Specific Configs

**Current Implementation:**
- Single CI workflow for PRs from developers
- Focus on code quality and formatting checks

**Future Implementation (Phase 3):**
- **Development PRs:** Current workflow (linting, formatting, unit tests)
- **Production Merges:** Enhanced workflow with regression testing
- **Deployment Pipeline:** Full E2E testing and security scans

**Approach:**
1. Start with single workflow for all PRs
2. Later add conditional steps based on target branch
3. Implement regression testing for main branch merges

---

## 🚀 Future Enhancements (Optional)

### 🔒 Pre-commit Hooks (Optional)
Use `husky` + `lint-staged` to automatically run checks before commits.

### 📊 Metrics & Success Tracking (Optional)
- [ ] CI adoption rate tracking
- [ ] Time to first CI pass metrics
- [ ] Error frequency analysis
- [ ] Test coverage trends

### 🔄 Advanced CI/CD (Future)
- [ ] Dependabot for dependency updates
- [ ] CodeQL for security scanning
- [ ] Deployment previews for PRs
- [ ] Automated release management

---

## 🧩 Strategic Clarifications

### 💡 Why CI doesn't auto-fix (and shouldn't):

Auto-fixing in CI creates Git discrepancies. Developers should fix **locally**, learn from errors, and recommit. CI ensures accountability without intruding on authoring flow. That's the "gentle enforcement."

### 🎯 Teaching vs Protecting Codebase

You can absolutely achieve both:

- **Protect** with linting/formatting + CI blocking
- **Teach** via transparent errors and README culture tips

Even volunteers can benefit from learning better coding habits through your tooling. It adds value to their time and respects the codebase.

### 🧱 Defining & Communicating Standards

1. **Define:** Codify in linting rules and formatting config. That's your technical baseline.
2. **Communicate:** Add usage notes in the README, onboarding doc, and checklist steps.
3. **Enforce (automate):** Via CI pipeline (already in place) and optionally via Husky.
4. **Guide team:** Offer framework-specific rules that reflect clean architecture and best practices.

If Asinsala or Christian want to extend this later (e.g. enforce scoped styles or test coverage), you'll already have the framework to plug those in.
