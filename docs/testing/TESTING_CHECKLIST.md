# Manual Testing Checklist

---

## Testing Overview

**Objectives:**
1. Verify all three user roles (Admin, Teacher, Student) work correctly
2. Test CRUD operations (Create, Read, Update, Delete)
3. Verify rich text editor works without freezing
4. Check RTL/Arabic support
5. Test mobile responsiveness
6. Ensure no console errors

**Test Environment:**
- Frontend: http://localhost:3000 (redirects to /ar for Arabic)
- Backend: http://127.0.0.1:8090 (PocketBase)
- Default credentials: admin@school.edu / Admin@12345

---

## Phase 1: Admin User Testing

### 1.1 Admin Login and Dashboard
- [ ] Login with admin credentials
- [ ] Verify admin dashboard loads
- [ ] Check no console errors
- [ ] Verify page load time is acceptable

### 1.2 Users Management
- [ ] Navigate to Admin > Users
- [ ] Verify user list displays
- [ ] Create a new user (fill form, click Save)
- [ ] Verify new user appears in list
- [ ] Edit existing user
- [ ] Delete a user (confirm dialog appears)

### 1.3 Settings Page
- [ ] Navigate to Admin > Settings
- [ ] Edit school name/settings
- [ ] Click Save
- [ ] Verify changes persist on reload

### 1.4 Subjects/Exams Management
- [ ] Navigate to Admin > Subjects/Exams
- [ ] Create new subject
- [ ] Edit subject
- [ ] Delete subject

### 1.5 Students and Sections
- [ ] Navigate to Admin > Students
- [ ] View student list
- [ ] Navigate to Admin > Sections
- [ ] Verify sections display correctly

### 1.6 Announcements (Admin)
- [ ] Navigate to Admin > Announcements
- [ ] Create announcement with rich text
- [ ] Verify it displays in list

**Admin Testing Status:** PASS / FAIL

---

## Phase 2: Teacher User Testing

### 2.1 Teacher Login and Dashboard
- [ ] Login with teacher credentials
- [ ] Verify teacher dashboard loads
- [ ] Check no console errors

### 2.2 Homework Page
- [ ] Navigate to Teacher > Homework
- [ ] Create homework (title, section, subject, due date, rich text)
- [ ] Edit existing homework
- [ ] Delete homework

### 2.3 Materials Page
- [ ] Navigate to Teacher > Materials
- [ ] Create material with rich text
- [ ] Verify material appears in list

### 2.4 Quizzes Page
- [ ] Navigate to Teacher > Quizzes
- [ ] Create quiz with questions
- [ ] View quiz results

### 2.5 Announcements (Teacher)
- [ ] Navigate to Teacher > Announcements
- [ ] Create announcement with rich text

**Teacher Testing Status:** PASS / FAIL

---

## Phase 3: Student User Testing

### 3.1 Student Login and Dashboard
- [ ] Login with student credentials
- [ ] Verify student dashboard loads
- [ ] Check no console errors

### 3.2 Homework (Student View)
- [ ] Navigate to Student > Homework
- [ ] View assigned homework
- [ ] Submit homework (if online submission)

### 3.3 Materials (Student View)
- [ ] Navigate to Student > Materials
- [ ] View materials
- [ ] Check rich content renders correctly

### 3.4 Quizzes (Student View)
- [ ] Navigate to Student > Quizzes
- [ ] Take a quiz
- [ ] View quiz results

### 3.5 Announcements (Student View)
- [ ] Navigate to Student > Announcements
- [ ] View announcements
- [ ] Add reactions/comments

**Student Testing Status:** PASS / FAIL

---

## Phase 4: Feature Testing

### 4.1 Rich Text Editor
- [ ] Create content with rich text
- [ ] Format text: Bold, Italic, Underline
- [ ] Add headings and lists
- [ ] Test text alignment
- [ ] Save and reload - verify formatting persists
- [ ] No browser freezing or CPU spike

### 4.2 Search/Filter
- [ ] Test search functionality on list pages
- [ ] Test filter dropdowns
- [ ] Verify results update correctly

### 4.3 Form Validation
- [ ] Submit form with empty required fields
- [ ] Verify validation messages appear
- [ ] Fill correctly and submit successfully

### 4.4 Dropdowns and Selects
- [ ] Test all dropdown menus
- [ ] Verify options load
- [ ] Can select items
- [ ] Selected value displays correctly

### 4.5 Date Picker
- [ ] Click date input fields
- [ ] Select dates
- [ ] Verify date displays in correct format

**Feature Testing Status:** PASS / FAIL

---

## Phase 5: UI/UX Testing

### 5.1 RTL/Arabic Support
- [ ] Switch language to Arabic
- [ ] Check page layout (should be right-to-left)
- [ ] Verify text is readable
- [ ] Check form alignment
- [ ] Navigation menus align correctly

### 5.2 Mobile Responsiveness
- [ ] Test at: 375px, 768px, 1024px widths
- [ ] Text readable
- [ ] Buttons clickable
- [ ] Forms functional
- [ ] Navigation accessible
- [ ] Dropdowns work

### 5.3 Accessibility
- [ ] Tab through form fields with keyboard
- [ ] Navigate without mouse
- [ ] Check buttons have focus states
- [ ] Try dropdown navigation with arrow keys

### 5.4 Visual Quality
- [ ] Check spacing and alignment
- [ ] Verify colors display correctly
- [ ] Check icon rendering
- [ ] Look for visual glitches

**UI/UX Testing Status:** PASS / FAIL

---

## Phase 6: Error Handling

### 6.1 Console Errors
- [ ] Open browser DevTools Console tab
- [ ] Should see NO errors (warnings are acceptable)
- [ ] Navigate through all pages
- [ ] Check for infinite loop messages

### 6.2 Network Errors
- [ ] Open DevTools Network tab
- [ ] Check for failed requests
- [ ] Verify API calls succeed (status 200/201)

### 6.3 Loading States
- [ ] Verify loading spinners show during data fetch
- [ ] Verify button state changes during submission
- [ ] Verify confirmation dialogs appear for destructive actions

**Error Handling Status:** PASS / FAIL

---

## Phase 7: Performance

### 7.1 Page Load Times
- [ ] Measure page load times (target: under 2 seconds)
- [ ] Check for long tasks in Performance tab

### 7.2 Memory Usage
- [ ] Check browser RAM usage
- [ ] Navigate between pages - RAM should not continuously grow

### 7.3 Interactions
- [ ] Button clicks respond instantly
- [ ] Form input feels responsive
- [ ] No lag when typing
- [ ] Dropdowns open smoothly

**Performance Status:** PASS / FAIL

---

## Phase 8: Regression Testing

### 8.1 Previous Fixes Still Work
- [ ] RichEditor does not freeze on Create
- [ ] Memory usage stays reasonable
- [ ] No infinite update errors
- [ ] Pages load without infinite loops

### 8.2 No New Errors Introduced
- [ ] Check console for new errors
- [ ] Verify all basic flows still work
- [ ] Test login, dashboard, CRUD operations

**Regression Testing Status:** PASS / FAIL

---

## Overall Testing Summary

| Phase | Status | Notes |
|-------|--------|-------|
| Admin User | PASS / FAIL | |
| Teacher User | PASS / FAIL | |
| Student User | PASS / FAIL | |
| Features | PASS / FAIL | |
| UI/UX | PASS / FAIL | |
| Error Handling | PASS / FAIL | |
| Performance | PASS / FAIL | |
| Regression | PASS / FAIL | |

---

## Success Criteria

All tests PASS if:
- No console errors (warnings are acceptable)
- All CRUD operations work (Create, Read, Update, Delete)
- Rich text editor works without freezing
- Forms submit successfully
- Pages load in acceptable time
- RAM usage stays reasonable
- Mobile responsive
- RTL/Arabic displays correctly
- All three user roles function properly