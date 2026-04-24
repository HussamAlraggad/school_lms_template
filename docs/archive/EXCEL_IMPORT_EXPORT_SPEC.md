# Student Bulk Import/Export Specification

This document specifies the feature for importing students from spreadsheet files and exporting data.

---

## Overview

The bulk import feature allows administrators to:
1. Upload a CSV, XLSX, XLS, or ODS file containing student names
2. Review and edit the parsed data before importing
3. Automatically generate email addresses and passwords from Arabic names
4. Assign students to class sections
5. Create all student accounts in PocketBase

---

## Import Process

### Step 1: Upload File
- Accept file formats: CSV, XLSX, XLS, ODS
- Maximum file size: 5MB
- Required column: Arabic name (column header must contain "الاسم" or "Name")
- Optional columns: English name, Section ID

### Step 2: Review and Edit
- Display parsed data in an editable table
- Each row shows: Arabic name, generated English name, generated email
- Admin can edit any field before import
- Validation errors highlighted in red

### Step 3: Assign Sections
- Select a section for all students, or
- Assign sections individually per student
- Section dropdown populated from class_sections collection

### Step 4: Import
- Create student accounts in PocketBase
- Generate random passwords for each student
- Display summary: created count, error count
- Option to download generated credentials as CSV

---

## Email Generation

Emails are generated from Arabic names using transliteration:

1. Transliterate Arabic name to Latin characters
2. Generate 8-character email base from first + last name
3. Append domain (@school.edu by default, configurable)
4. If email already exists, append numeric suffix

Example:
- Arabic name: "أحمد محمد" → Email: ahmedmo@school.edu
- If ahmedmo exists → ahmedm1@school.edu

---

## Password Generation

- Auto-generated passwords are 12 characters
- Mix of uppercase, lowercase, numbers, and symbols
- Displayed once during import (not stored in plain text)
- Students should change password on first login

---

## Export Feature

### Student Export
- Export student list as CSV or XLSX
- Columns: Arabic name, English name, email, section, grade
- Filter by section, grade, or all students

### Data Export
- Export any collection data as CSV
- Useful for reports and data analysis

---

## Error Handling

| Error | Display | Resolution |
|-------|---------|------------|
| Invalid file format | "Unsupported file format. Please upload CSV, XLSX, XLS, or ODS." | Upload correct format |
| File too large | "File exceeds maximum size of 5MB." | Reduce file size |
| Missing name column | "Could not find a name column. Ensure your file has a column with header containing 'الاسم' or 'Name'." | Fix column header |
| Duplicate email | "Email already exists: {email}" | Edit email or skip row |
| Empty row | "Row {n} is empty" | Remove empty rows |
| Invalid section | "Section not found" | Select valid section |

---

## Technical Implementation

### File Parsing
- CSV: Line-by-line parsing with proper quote handling
- XLSX/XLS/ODS: SheetJS (xlsx) library for spreadsheet parsing
- Auto-detect file type by extension and MIME type

### Transliteration
- Arabic-to-Latin character mapping
- Handles Arabic letter forms (initial, medial, final, isolated)
- Configurable email domain via platform settings

### Validation
- Required fields: Arabic name
- Email format validation
- Duplicate email detection
- Section existence check