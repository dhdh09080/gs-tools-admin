# GS Tools Admin — Project Status

Last updated: 2026-10-12

## Current production
- URL: https://gs-tools-admin.vercel.app/
- Repository: `dhdh09080/gs-tools-admin`
- Branch: `main`
- Vercel: GitHub main auto-deploy is connected.

## Frontend structure
- `index.html` — login/signup shell + admin shell; external assets referenced with cache-busting.
- `styles.css` — Linear/Vercel-inspired light admin UI.
- `app.js` — Supabase auth/data logic, wrapped in an IIFE to avoid browser global name collisions.
- `vercel.json` — no-store cache headers while frontend is being stabilized.

## Implemented
### Authentication
- Login with Supabase email/password.
- Public signup UI.
- Signup fields: name, phone, department, email, password.
- Pending users are blocked from the workspace until approved.

### Roles
- `master`
- `editor`
- `viewer`
- User status: `pending / approved / rejected / suspended`

### Member management
- Master-only member-management menu.
- Approve/reject pending members.
- Viewer/Editor role assignment.
- Suspend/restore.
- Private admin memo.

### Feedback
- Dashboard summary.
- Recent feedback rows open the selected inbox item.
- Program/status filtering.
- Editor/Master status changes and admin notes.
- Viewer read-only behavior.

### Notices
- List notices.
- Editor/Master can create notices.
- Notice can target all programs or one program.

### Programs / releases
- Program list.
- Release upload to Supabase Storage.
- Release candidate status: pending.
- Master approve/reject.
- Approved release updates program latest version.
- Keep latest + immediately previous approved installation files.
- Older release records remain in DB.

## Supabase
Project: `GS Tools Admin`
Project ref: `vzfujgzhxtrtqrzqwtsq`

Main tables:
- `programs`
- `feedback`
- `error_logs`
- `notices`
- `releases`
- `profiles`
- legacy `admin_users`

Storage:
- `feedback-attachments`
- `release-files`

Security:
- RLS enabled on application tables.
- Helper permission functions exist for member/admin/master access.
- Release and member approval policies have been added.

## Master account
- Master email: `daheekoo@gsenc.com`
- Intended method: create directly in Supabase Authentication → Users.
- The profile trigger promotes this email to `master + approved`.
- Do not create the Master account through public signup.
- Master password must remain only in Supabase Auth.

## Current verification state
- Frontend had a sequence of browser initialization errors caused by duplicated/global declarations and classic-script top-level await.
- The app script has since been isolated in an IIFE and externalized.
- Current visible build marker intended for production: `build 2026.10.10-3`.
- Full production login after this stabilization change has not yet been confirmed by the user in chat.
- Before new feature work, verify the deployed build and login once. If an initialization error remains, inspect the deployed asset/source first rather than making speculative edits.

## Next recommended checks
1. Confirm production shows build `2026.10.10-3`.
2. Confirm login button executes without a JavaScript initialization error.
3. Create/verify Master Auth user in Supabase.
4. Login as Master and confirm `회원관리` appears.
5. Test one dummy signup → pending → Master approval → login.
6. Test one dummy release upload → Master approval → latest-version update.
7. Test old release-file cleanup only after two approved releases exist.

## Known future work
- Automatic email sending when feedback is resolved (currently mailto/manual flow unless separately wired).
- Audit/activity history per member and release can be added later.
- Additional program-specific permissions can be added if needed.
