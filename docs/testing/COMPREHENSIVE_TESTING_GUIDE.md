# Comprehensive Testing Guide

This guide provides detailed test cases for all platform functionality across all user roles.

---

## Prerequisites

- PocketBase running at http://127.0.0.1:8090
- Frontend running at http://localhost:3000
- Test users created (Admin, Teacher, Student)
- Seed data loaded (subjects, sections)

---

## Part 1: Authentication

### 1.1 Login
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to /login | Login form displays |
| 2 | Enter valid admin credentials | Redirected to admin dashboard |
| 3 | Enter valid teacher credentials | Redirected to teacher dashboard |
| 4 | Enter valid student credentials | Redirected to student dashboard |
| 5 | Enter invalid credentials | Error message displayed |
| 6 | Leave email empty | Validation message shown |
| 7 | Leave password empty | Validation message shown |

### 1.2 Logout
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Click sign out button | Redirected to login page |
| 2 | Try accessing dashboard directly | Redirected to login |

### 1.3 Role-Based Access
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Admin tries to access /dashboard/teacher | Redirected to /dashboard/admin |
| 2 | Teacher tries to access /dashboard/admin | Redirected to /dashboard/teacher |
| 3 | Student tries to access /dashboard/admin | Redirected to /dashboard/student |

---

## Part 2: Admin Dashboard

### 2.1 Admin Overview
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Login as admin | Overview page with stat cards |
| 2 | Check stat cards | Display user counts, class counts |
| 3 | Check announcements section | Announcements list displays |

### 2.2 Admin Users Management
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Admin > Users | Teachers and Students tabs |
| 2 | Click Teachers tab | Teacher list with filters |
| 3 | Click Students tab | Student list with filters |
| 4 | Create new teacher | Form opens, fill and save |
| 5 | Edit existing teacher | Form pre-populates, save changes |
| 6 | Delete teacher | Confirmation dialog, then removed |
| 7 | Search/filter users | Results update dynamically |

### 2.3 Admin Sections
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Admin > Sections | Sections grouped by grade |
| 2 | Create new section | Form opens, fill grade/section names |
| 3 | Edit section | Pre-populated form, save changes |
| 4 | Delete section | Confirmation, cascade deletes related data |

### 2.4 Admin Subjects
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Admin > Subjects | Subject list |
| 2 | Create subject | Fill name_ar, name_en, code |
| 3 | Edit subject | Pre-populated form |
| 4 | Delete subject | Confirmation, cascade deletes |

### 2.5 Admin Announcements
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Admin > Announcements | Announcements list |
| 2 | Create global announcement | Rich text editor, scope=global |
| 3 | Create section announcement | Scope=section, select section |
| 4 | Edit announcement | Pre-populated rich text |
| 5 | Delete announcement | Confirmation dialog |

### 2.6 Admin Settings
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Admin > Settings | Settings page with accordions |
| 2 | Change school name | Save, verify persists on reload |
| 3 | Toggle feature flags | Comments, reactions, quizzes toggles |
| 4 | Change password | Current password, new password |

---

## Part 3: Teacher Dashboard

### 3.1 Teacher Overview
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Login as teacher | Overview with stat cards |
| 2 | Check stat cards | Subject count, student count, homework stats |

### 3.2 Teacher Materials
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Teacher > Materials | Materials list |
| 2 | Create material | Title, section, subject, rich text body |
| 3 | Upload file attachment | File upload works |
| 4 | Add external link | Link URL field |
| 5 | Edit material | Pre-populated form |
| 6 | Delete material | Confirmation dialog |

### 3.3 Teacher Homework
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Teacher > Homework | Homework list |
| 2 | Create homework | Title, section, subject, due date, type |
| 3 | View submissions | Expand to see student submissions |
| 4 | Grade submission | Enter grade and feedback |
| 5 | Edit homework | Pre-populated form |
| 6 | Delete homework | Confirmation dialog |

### 3.4 Teacher Quizzes
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Teacher > Quizzes | Quiz list |
| 2 | Create quiz | Title, section, subject, time limit |
| 3 | Add questions | Question text, options, correct answer |
| 4 | View quiz results | Student attempts with scores |
| 5 | Edit quiz | Pre-populated form |
| 6 | Delete quiz | Confirmation dialog |

---

## Part 4: Student Dashboard

### 4.1 Student Overview
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Login as student | Overview with stat cards |
| 2 | Check stat cards | Subject count, homework count, quiz count |
| 3 | Check exam schedule | Upcoming exams section |

### 4.2 Student Materials
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Student > Materials | Materials for student's section |
| 2 | Filter by subject | Dropdown filter |
| 3 | Expand material | Full rich content, links, files |
| 4 | Download attachment | File downloads |
| 5 | Add comment | Comment appears |

### 4.3 Student Homework
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Student > Homework | Homework for student's section |
| 2 | View homework details | Description, due date, type |
| 3 | Submit online homework | Rich text editor for submission |
| 4 | View grade/feedback | Grade and feedback from teacher |

### 4.4 Student Quizzes
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to Student > Quizzes | Available quizzes |
| 2 | Start quiz | Timer starts, questions display |
| 3 | Answer questions | Navigate between questions |
| 4 | Submit quiz | Score displayed |
| 5 | Auto-submit on time expiry | Quiz submitted automatically |

---

## Part 5: Bilingual Support

### 5.1 Language Switching
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Switch from Arabic to English | All text switches to English |
| 2 | Switch from English to Arabic | All text switches to Arabic |
| 3 | Check RTL layout in Arabic | Right-to-left alignment |
| 4 | Check LTR layout in English | Left-to-right alignment |
| 5 | Verify form labels in both languages | Labels display correctly |

---

## Part 6: Error Handling

### 6.1 Form Errors
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Submit form with empty required fields | Validation error messages |
| 2 | Submit form with invalid email | Email validation error |
| 3 | Create duplicate record | Error message displayed |
| 4 | Network error during submission | Error alert with retry option |

### 6.2 Loading States
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to data-heavy page | Loading spinner displays |
| 2 | Submit form | Button shows loading state |
| 3 | Delete record | Confirmation dialog appears |

---

## Part 7: Accessibility

### 7.1 Keyboard Navigation
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Tab through form fields | Each field receives focus |
| 2 | Enter to submit form | Form submits |
| 3 | Escape to close dialog | Dialog closes |
| 4 | Arrow keys in dropdown | Navigate options |

### 7.2 Screen Reader
| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate with screen reader | Elements announced correctly |
| 2 | Check button labels | Descriptive labels |
| 3 | Check form labels | Associated with inputs |