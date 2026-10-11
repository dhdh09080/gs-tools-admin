# GS Tools Admin — Project Instructions

## 1. Source of truth
- Repository: `dhdh09080/gs-tools-admin`
- Branch: `main`
- Always read the latest `main` before changing code.
- GitHub code and Supabase schema are authoritative. Do not rely on old chat code or screenshots when they conflict.
- Read `PROJECT_STATUS.md` before starting work.
- After meaningful changes, update `PROJECT_STATUS.md`.

## 2. Stack
- Frontend: static HTML / CSS / JavaScript
- Auth / DB / Storage: Supabase
- Hosting: Vercel
- Source control: GitHub
- Production URL: https://gs-tools-admin.vercel.app/

## 3. Architecture rules
- `index.html`: markup only plus external script/style references and minimal bootstrap/error handling.
- `styles.css`: all UI styling.
- `app.js`: all application logic.
- Do not duplicate application JS or CSS inside `index.html`.
- Avoid global-scope name collisions. Keep app code isolated in an IIFE/module scope.
- Supabase client variable: `sbClient`.
- Never rename or rewrite the literal Supabase host string accidentally.
- Do not put service-role keys or plaintext passwords in frontend code.

## 4. Authentication and roles
- Public signup is allowed.
- Signup fields: name, phone, department, email, password.
- New users default to `viewer + pending`.
- Pending users cannot access the admin workspace until approved.
- Roles:
  - `master`: full access, member approval, role/status changes, release final approval.
  - `editor`: handle feedback, post notices, upload release candidates.
  - `viewer`: read-only access.
- Master account email: `daheekoo@gsenc.com`.
- Master account is created directly in Supabase Auth, not via public signup.
- Passwords are managed only by Supabase Auth and must never be stored in app tables.

## 5. Member management
- Master can approve/reject new members.
- Master can change Viewer/Editor role.
- Master can suspend/restore accounts.
- Master can store a private admin memo per user.
- Historical activity should remain even if an account is suspended.

## 6. Release workflow
- Editor/Master can upload a release candidate.
- New release status: `pending`.
- Only Master can approve or reject.
- A release becomes the program's latest version only after Master approval.
- Keep physical installation files for only:
  1. latest approved release
  2. immediately previous approved release
- Older release DB history remains; only old files are removed.
- Never delete old files before a new upload/approval has succeeded.

## 7. Feedback / notices
- Dashboard recent-feedback rows must open the matching feedback detail.
- Inbox must support program and status filtering.
- Editors/Master can update feedback status and admin notes.
- Viewer is read-only.
- Notices can target all programs or one program.

## 8. Design
- Direction: Linear × Vercel with Apple-like spacing/typography.
- Functional admin tool first: clean white/near-white surfaces, thin borders, restrained shadows, rounded cards.
- Do not redesign unrelated screens while fixing a bug.
- Preserve visual consistency across login, dashboard, inbox, releases, and member management.

## 9. Development workflow
1. Read latest repository files relevant to the task.
2. Reproduce/identify the root cause before editing.
3. Change the smallest necessary surface.
4. Run static checks for:
   - JS parse errors
   - duplicate declarations
   - stale references
   - wrong Supabase URL
   - top-level await in classic scripts
   - duplicated inline/external app code
5. Commit to `main` with a descriptive message.
6. Let Vercel auto-deploy.
7. Update `PROJECT_STATUS.md`.

## 10. Token-saving rule for ChatGPT
- Do not recap the full project unless asked.
- Do not paste whole source files into chat unless necessary.
- Prefer: inspect latest GitHub → edit exact file → summarize only changed behavior.
- In a new chat, the user may say:
  "최신 main과 PROJECT_STATUS.md 먼저 확인하고 이번 작업만 진행해줘."
  Treat that as sufficient context.
