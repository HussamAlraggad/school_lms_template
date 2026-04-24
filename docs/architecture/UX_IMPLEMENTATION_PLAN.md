# UX Architecture Implementation Guide

This guide documents the architectural patterns and implementation methodology for building a clean, maintainable school learning platform UI.

---

## Core Principles

1. **State Consolidation:** Use custom hooks to reduce useState calls from 15-23 per page to 5-6
2. **Error Visibility:** Every form submission must display errors to the user
3. **Accessibility:** WCAG 2.1 compliance for all interactive elements
4. **Performance:** Code splitting, lazy loading, and efficient data fetching
5. **Bilingual Support:** RTL-first design with Arabic/English locale switching

---

## Custom Hooks for State Management

All hooks are located in `frontend/src/lib/hooks/` and exported via `index.ts`.

### `useCrudState` - CRUD UI State

Consolidates 5 common CRUD UI states into one hook:

```typescript
import { useCrudState } from "@/lib/hooks";

const { state, setShowCreate, setEditingId, setIsLoading, setError, clearError } = useCrudState<User>();

// state contains: showCreate, editingId, isLoading, error, expandedId
// Each setter is memoized with useCallback
// reset() returns all values to initial state
```

**When to use:** Any page with create/edit/delete operations.

### `useFormState` - Form Data and Validation

Consolidates form data, errors, and touched state:

```typescript
import { useFormState } from "@/lib/hooks";

const { state, setFieldValue, setFieldError, setFieldTouched, setData, setErrors, reset } = useFormState<CreateUserForm>(initialData);

// state contains: data, errors, touched
// setFieldValue("name_ar", value) - update a single field
// setFieldError("email", "Required") - set a field error
// reset() - return to initial state
```

**When to use:** Any form with multiple fields.

### `useFilterState` - Search, Filter, and Pagination

Consolidates search, filter, sort, and pagination:

```typescript
import { useFilterState } from "@/lib/hooks";

const { state, setSearchTerm, setRoleFilter, setSortBy, setPage } = useFilterState();

// state contains: searchTerm, roleFilter, sortBy, page, perPage
// Auto-resets page to 1 when search/filter/sort changes
```

**When to use:** Any list page with search, filters, or pagination.

### `useTabState` - Tab Navigation

Consolidates active tab and per-tab data:

```typescript
import { useTabState } from "@/lib/hooks";

const { state, setActiveTab, setTabData, updateTabData } = useTabState('overview');

// state contains: activeTab, tabData (Record<string, any>)
// updateTabData merges partial data into the current tab's data
```

**When to use:** Any page with tab navigation.

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

## Refactoring Methodology

### Before Starting a Refactor

1. Read the current page code
2. Identify state groups (form data, CRUD UI, filters, tabs)
3. Plan which hooks to use for each group
4. Test the current page in browser (both locales)

### During Refactoring

1. Import hooks from `@/lib/hooks`
2. Replace useState calls with appropriate hooks
3. Update all setters to use hook methods
4. Add error handling wrapper (try-catch + setError)
5. Add FormErrorAlert to form JSX

### After Refactoring

1. Verify page builds (`npm run build`)
2. Test form create/edit/delete
3. Test error scenarios
4. Test search/filter functionality
5. Test in both locales (AR/EN)
6. Test on mobile viewport

### State Health Guidelines

- **Excessive:** 7+ states per component (refactor needed)
- **Healthy:** 3-5 states per component (good balance)
- **Over-consolidated:** 1 state for everything (only if truly related)

---

## 5-Phase Implementation Plan

### Phase 1: State Management Refactoring
- Create custom hooks (useCrudState, useFormState, useFilterState, useTabState)
- Apply hooks to highest-complexity pages first
- Target: 15-23 states per page reduced to 5-7

### Phase 2: Accessibility
- Add ARIA labels to all interactive elements
- Ensure keyboard navigation works for all components
- Add focus management for modals and dialogs
- Test with screen readers

### Phase 3: Testing
- Set up Jest with React Testing Library
- Write unit tests for custom hooks
- Write integration tests for critical user flows
- Target: 70%+ coverage

### Phase 4: Component Extraction
- Extract reusable patterns into composite components
- Create FormDialog, DataTable, FilterBar components
- Standardize CRUD page patterns

### Phase 5: Performance
- Implement code splitting for dashboard pages
- Add lazy loading for heavy components (RichEditor)
- Configure React Query for efficient caching
- Optimize bundle size with dynamic imports

---

## Maintenance Notes

1. **Always use hooks for state** - Never scatter 10+ useState calls in a page
2. **Always wrap form submissions in error handling** - Users must see errors
3. **Display errors to users** - Use FormErrorAlert for all form errors
4. **Count states before/after refactor** - Track progress toward healthy state counts
5. **Test all scenarios** - Happy path, error path, both locales, mobile viewport