# Common React State Management Anti-Patterns and Solutions

This document catalogs the 5 most common state management anti-patterns found in React applications, with before/after code examples and recommended solutions.

---

## Pattern 1: Form State Explosion (30% of violations)

**Problem:** Each form field gets its own useState, leading to 8+ separate states for a single form.

```typescript
// BAD: 8 separate states for one form
const [name, setName] = useState("");
const [email, setEmail] = useState("");
const [password, setPassword] = useState("");
const [nameError, setNameError] = useState("");
const [emailError, setEmailError] = useState("");
const [nameTouched, setNameTouched] = useState(false);
const [emailTouched, setEmailTouched] = useState(false);
const [saving, setSaving] = useState(false);

// Updates are verbose:
setForm(f => ({ ...f, name_ar: value }));
setErrors(e => ({ ...e, email: "Required" }));
setTouched(t => ({ ...t, email: true }));
```

```typescript
// GOOD: 1 state with useFormState hook
const { state, setFieldValue, setFieldError, setFieldTouched } = useFormState(initialData);
const { error, setError, clearError } = useFormError();

// Updates are clean:
setFieldValue("name_ar", value);
setFieldError("email", "Required");
```

**Solution:** Use `useFormState` hook from `@/lib/hooks`.

---

## Pattern 2: UI Toggle Explosion (25% of violations)

**Problem:** Multiple boolean states for show/hide, edit mode, expanded items.

```typescript
// BAD: Multiple visibility states
const [showForm, setShowForm] = useState(false);
const [editingId, setEditingId] = useState(null);
const [expandedItem, setExpandedItem] = useState(null);
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState("");
```

```typescript
// GOOD: Single UI state object with useCrudState hook
const { state, setShowCreate, setEditingId, setIsLoading, setError, clearError } = useCrudState();

// state contains: showCreate, editingId, isLoading, error, expandedId
```

**Solution:** Use `useCrudState` hook from `@/lib/hooks`.

---

## Pattern 3: Async Loading States (20% of violations)

**Problem:** Multiple loading/error states for different async operations.

```typescript
// BAD: Multiple loading states
const [loading, setLoading] = useState(false);
const [saving, setSaving] = useState(false);
const [deleting, setDeleting] = useState(false);
const [error, setError] = useState("");
```

```typescript
// GOOD: Consolidated with useCrudState
const { state, setIsLoading, setError, clearError } = useCrudState();

// Or use a dedicated async state pattern:
const [asyncState, setAsyncState] = useState({ loading: false, saving: false, deleting: false, error: "" });
```

**Solution:** Use `useCrudState` for CRUD operations, or consolidate async states into a single object.

---

## Pattern 4: Data Collections (15% of violations)

**Problem:** Separate state for each data collection with its own loading flag.

```typescript
// BAD: Separate state for each collection
const [materials, setMaterials] = useState([]);
const [announcements, setAnnouncements] = useState([]);
const [materialsLoading, setMaterialsLoading] = useState(false);
const [announcementsLoading, setAnnouncementsLoading] = useState(false);
```

```typescript
// GOOD: React Query for server state
const { data: materials, isLoading: materialsLoading } = useQuery({
  queryKey: ['materials'],
  queryFn: () => pb.collection('materials').getFullList()
});

const { data: announcements, isLoading: announcementsLoading } = useQuery({
  queryKey: ['announcements'],
  queryFn: () => pb.collection('announcements').getFullList()
});
```

**Solution:** Use React Query (TanStack Query) for server state. Local state should only handle UI concerns.

---

## Pattern 5: Edit Mode States (10% of violations)

**Problem:** Multiple edit-related states scattered across a component.

```typescript
// BAD: Multiple edit states scattered
const [editingId, setEditingId] = useState(null);
const [editForm, setEditForm] = useState({});
const [editError, setEditError] = useState("");
```

```typescript
// GOOD: Single edit state object or useCrudState
const { state, setEditingId, setError, clearError } = useCrudState();
// editingId tracks which item is being edited
// error handles edit errors
```

**Solution:** Use `useCrudState` for edit mode tracking, or consolidate edit states into a single object.

---

## Quick Reference: When to Use Each Hook

| Hook | Use Case | States Consolidated |
|------|----------|---------------------|
| `useCrudState` | CRUD operations (show/hide, edit, loading, error) | 5 states to 1 |
| `useFormState` | Form data, validation, touched tracking | 15+ states to 1 |
| `useFilterState` | Search, filters, sort, pagination | 4 states to 1 |
| `useTabState` | Tab navigation, per-tab data | 3 states to 1 |
| `useFormError` | Error/success messages with auto-dismiss | 2 states to 1 |

---

## Health Check: State Count Guidelines

- **Excessive:** 7+ states per component (refactor needed)
- **Healthy:** 3-5 states per component (good balance)
- **Over-consolidated:** 1 state for everything (only if truly related)

---

## Developer Commands

```bash
# Count useState in a file
grep -c "useState" path/to/file.tsx

# Find all excessive useState components
find frontend/src -name "*.tsx" -type f | while read f; do
  count=$(grep -c "useState" "$f" 2>/dev/null || echo 0);
  if [ "$count" -gt 6 ]; then echo "$count:$f"; fi;
done | sort -rn

# Check component size
wc -l frontend/src/app/[lang]/dashboard/admin/users/page.tsx
```