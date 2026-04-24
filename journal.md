# Project Journal

## Template Initialization

**Date:** Template initialized
**Status:** COMPLETE

### What was done:
- Copied the full project structure from the reference implementation
- Stripped all school-specific branding (school name, email domain, logo)
- Replaced with generic placeholders (School Name / اسم المدرسة, @school.edu)
- Set PocketBase URL to localhost:8090 for local development
- Removed production deployment configs
- Removed session/milestone documentation
- Kept architecture patterns, testing guides, and UX documentation (generalized)
- Created fresh template documentation

### What to customize before use:
1. Update `frontend/src/context/settings-context.tsx` default school names
2. Update `frontend/src/app/layout.tsx` metadata (title, description)
3. Update `frontend/src/lib/transliteration.ts` email domain
4. Update `frontend/src/dictionaries/ar.json` and `en.json` with your school's name
5. Update `backend/seed.js` with your admin credentials and school data
6. Add your school logo to `frontend/public/logo.webp`
7. Download PocketBase binary for your OS from https://pocketbase.io/docs/
8. Run `backend/seed.js` after starting PocketBase to create initial data

---

## Milestones

*(No milestones yet — add your project milestones below)*