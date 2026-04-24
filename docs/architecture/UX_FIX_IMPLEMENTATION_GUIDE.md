# UX Fix Implementation Guide

This guide documents the custom hooks, error handling patterns, and refactoring methodology used in the platform.

---

## Custom Hooks for State Management

All hooks are located in `frontend/src/lib/hooks/` and exported via `index.ts`.

### `useCrudState` - CRUD UI State (5 states to 1)

```typescript
import { useCrudState } from "@/lib/hooks";

const { state, setShowCreate, setEditingId, setIsLoading, setError, setExpandedId, clearError, reset } = useCrudState<User>();

// Consolidates: showCreate, editingId, isLoading, error, expandedId
// Each setter is memoized with useCallback
// reset() returns all values to initial state
```

### `useFormState` - Form Data and Validation (15+ states to 1)

```typescript
import { useFormState } from "@/lib/hooks";

const { state, setFieldValue, setFieldError, setFieldTouched, setData, setErrors, reset, clearErrors } = useFormState<CreateUserForm>(initialData);

// Consolidates: data, errors, touched
// setFieldValue("name_ar", value) - update a single field
// setFieldError("email", "Required") - set a field error
// reset() - return to initial state
```

### `useFilterState` - Search, Filter, Pagination (4 states to 1)

```typescript
import { useFilterState } from "@/lib/hooks";

const { state, setSearchTerm, setRoleFilter, setSortBy, setPage, setPerPage } = useFilterState();

// Consolidates: searchTerm, roleFilter, sortBy, page, perPage
// Auto-resets page to 1 when search/filter/sort changes
```

### `useTabState` - Tab Navigation (3 states to 1)

```typescript
import { useTabState } from "@/lib/hooks";

const { state, setActiveTab, setTabData, updateTabData } = useTabState('overview');

// Consolidates: activeTab, tabData (Record<string, any>)
// updateTabData merges partial data into the current tab's data
```

---

## Error Handling Pattern

### `FormErrorAlert` and `useFormError`

Located in `frontend/src/components/ui/form-alerts.tsx`:

```typescript
import { FormErrorAlert, useFormError } from "@/components/ui/form-alerts";

const { error, setError, clearError } = useFormError();

async function handleSubmit(e: React.FormEvent) {
  e.preventDefault();
  clearError();
  setSaving(true);
  try {
    await pb.collection("users").create(formData);
    closeForm();
  } catch (err: any) {
    setError(err?.message || "Failed to save");
  } finally {
    setSaving(false);
  }
}

// In JSX:
{error && <FormErrorAlert error={error} onDismiss={clearError} />}
```

**Rule:** Every form submission must have try-catch with visible error display.

---

## Refactoring Patterns

### Pattern 1: Replacing Scattered useState with useCrudState

**Before (5 separate states):**
```typescript
const [showCreate, setShowCreate] = useState(false);
const [editingId, setEditingId] = useState<string | null>(null);
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState("");
const [expandedId, setExpandedId] = useState<string | null>(null);
```

**After (1 hook):**
```typescript
const { state, setShowCreate, setEditingId, setIsLoading, setError, clearError } = useCrudState();
```

### Pattern 2: Replacing Form State with useFormState

**Before (5+ states per form):**
```typescript
const [form, setForm] = useState({ name_ar: "", name_en: "", email: "", ... });
const [errors, setErrors] = useState({ name_ar: "", email: "", ... });
const [touched, setTouched] = useState({ name_ar: false, email: false, ... });
```

**After (1 hook):**
```typescript
const { state: form, setFieldValue, setFieldError } = useFormState(initialData);
const { error, setError } = useFormError();
```

### Pattern 3: Adding Error Alerts to Forms

**Before (silent failure):**
```typescript
async function handleSubmit(e: React.FormEvent) {
  e.preventDefault();
  setSaving(true);
  try {
    await pb.collection("users").create(formData);
    closeForm();
  } finally {
    setSaving(false);
  }
  // Error? User sees nothing
}
```

**After (visible error):**
```typescript
const { error, setError, clearError } = useFormError();

async function handleSubmit(e: React.FormEvent) {
  e.preventDefault();
  clearError();
  setSaving(true);
  try {
    await pb.collection("users").create(formData);
    closeForm();
  } catch (err: any) {
    setError(err?.message || "Failed to save");
  } finally {
    setSaving(false);
  }
}

// In JSX:
{error && <FormErrorAlert error={error} onDismiss={clearError} />}
```

---

## Refactoring Roadmap

### Priority 1 (Critical) - Highest State Count Pages
1. Admin Users page (23 states) - Extract useFormState, useCrudState, useFilterState
2. Admin Settings page (19 states) - Extract useFormState, useTabState for accordions
3. Student Assessments page (18 states) - Extract useTabState, useCrudState

### Priority 2 (High) - Medium State Count Pages
4. Teacher Quizzes page (18 states)
5. Admin Subjects/Exams page (17 states)

### Priority 3 (Standard) - Lower State Count Pages
6. Apply error handling to all remaining form pages
7. Standardize CRUD page patterns

---

## Implementation Checklist

For each page refactoring:

```
Page: [page].tsx - X states -> Target Y states

BEFORE STARTING:
- [ ] Read current page code
- [ ] Identify state groups
- [ ] Plan hook assignments
- [ ] Test current page in browser

DURING REFACTORING:
- [ ] Import hooks from @/lib/hooks
- [ ] Replace useState calls with hooks
- [ ] Update all setters to use hook methods
- [ ] Add error handling wrapper (try-catch + setError)
- [ ] Add FormErrorAlert to form JSX

AFTER REFACTORING:
- [ ] Verify page builds (npm run build)
- [ ] Test form create/edit/delete
- [ ] Test error scenarios
- [ ] Test search/filter functionality
- [ ] Test in both locales (AR/EN)
- [ ] Test on mobile viewport

SUCCESS METRICS:
- [ ] useState calls reduced by 60-75%
- [ ] All errors visible to user
- [ ] No TypeScript errors
- [ ] No runtime errors
```

---

## Maintenance Notes

1. **Always use hooks for state** - Form data uses useFormState, CRUD UI uses useCrudState, etc.
2. **Always wrap form submissions in error handling** - Users must see errors
3. **Display errors to users** - Use FormErrorAlert for all form errors
4. **Count states before/after refactor** - Track progress toward healthy state counts
5. **Test all scenarios** - Happy path, error path, both locales, mobile viewport