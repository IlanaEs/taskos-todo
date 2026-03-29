# Spike: TaskOS — Todo & Notes Dashboard

**Date:** March 29, 2026
**Author:** TaskOS Team
**Status:** Complete

---

## 1. Background

We are building a personal task management web app called **TaskOS** — a single-page dashboard for managing tasks with categories, priorities, deadlines, and notes. This spike investigates the key technical decisions before scaling the app further.

---

## 2. Core Features

| Feature | Description |
|---|---|
| Add task | Title + optional notes/description |
| Edit task | Inline editing of title and notes |
| Complete task | Toggle done/undone |
| Delete task | Remove a single task |
| Clear completed | Bulk delete all done tasks |
| Categories | Work, Personal, Shopping, Health, Other |
| Priority levels | Low / Medium / High |
| Deadline | Optional date with overdue indicator |
| Filters | All / Active / Completed + by category |
| Progress bar | Visual % of completed tasks |
| Dark mode | Toggle with persistence |
| Error handling | Empty input, title too long, notes too long |

---

## 3. Additional Features (Considered)

| Feature | Decision |
|---|---|
| User accounts / login | ❌ Not needed for now |
| Backend / database | ❌ Not needed for now |
| Push notifications / reminders | ⏳ Future consideration |
| Drag & drop reordering | ⏳ Future consideration |
| Search | ⏳ Future consideration |
| Export (PDF / CSV) | ⏳ Future consideration |
| Subtasks | ⏳ Future consideration |

---

## 4. Main User Flows

### Adding a Task
1. User types a title in the input field
2. Optionally adds notes, selects category, priority, and deadline
3. Clicks "+ Add" or presses Enter
4. Card appears in the grid with animation

### Editing a Task
1. User clicks the ✎ edit button on a card
2. Title and notes become editable inline
3. User saves or cancels

### Completing a Task
1. User clicks ✓ on a card
2. Card fades and gets strikethrough
3. Stats and progress bar update in real time

### Error Flow
1. User tries to submit an empty title → red error banner appears
2. User types more than 100 chars in title → error shown
3. User types more than 500 chars in notes → error shown
4. Error auto-dismisses after 3.5 seconds

---

## 5. System Architecture

### Current Architecture
```
Browser (Single Page App)
├── index.html         — full app (HTML + CSS + JS in one file)
├── localStorage       — persists tasks and theme preference
└── No backend         — fully client-side
```

### Data Model
```json
{
  "id": 1711700000000,
  "title": "Design new landing page",
  "notes": "Focus on mobile first",
  "priority": "high",
  "cat": "work",
  "deadline": "2026-04-01",
  "done": false,
  "time": "10:30 AM"
}
```

### Storage
- **Engine:** `localStorage`
- **Key:** `taskos-v3`
- **Limit:** ~5MB (sufficient for personal use)
- **Theme:** stored separately under `taskos-theme`

---

## 6. UI Layout & Style

### Layout
- **Desktop:** 3-column masonry card grid
- **Tablet:** 2-column grid
- **Mobile:** 1-column stack

### Design Language
- Glassmorphism — frosted glass cards with blur
- Pastel gradient background (purple → blue → pink)
- Animated background blobs
- Dark mode — deep purple/navy tones
- Font: system-ui / SF Pro (iOS-native feel)
- Card colors per category (soft tints)

### Key Components
| Component | Description |
|---|---|
| Top bar | Logo + dark mode toggle |
| Stats row | Total / Done / Remaining |
| Progress bar | % complete with gradient fill |
| Category tabs | Filter by category with counts |
| Add form | Title, notes, category, priority, deadline |
| Card | Note-style tile with edit/complete/delete |
| Filter row | All / Active / Completed + Clear button |

---

## 7. Language, Libraries & Tools

| Item | Choice | Reason |
|---|---|---|
| Language | HTML + CSS + JavaScript | No build step, runs anywhere |
| Framework | None (Vanilla JS) | Simple enough, no overhead |
| Styling | Pure CSS with CSS variables | Easy theming, no dependencies |
| Storage | localStorage | No backend needed |
| Deployment | GitHub Pages / Netlify | Free, instant |
| Version control | Git + GitHub | Standard |

---

## 8. Exceptions & Error States

| Scenario | Handling |
|---|---|
| Empty task title | Show red error banner: "Task title cannot be empty." |
| Title > 100 chars | Show error: "Title is too long (max 100 characters)." |
| Notes > 500 chars | Show error: "Notes are too long (max 500 characters)." |
| Empty title on edit | Show error, block save |
| localStorage full | Silent fail (edge case, unlikely for personal use) |
| No tasks in filter | Show empty state illustration + message |

---

## 9. Success Criteria

The app is considered **working correctly** when:

- [ ] A user can add a task with title, notes, category, priority, and deadline
- [ ] A user can edit any task inline
- [ ] A user can mark tasks as done and undo
- [ ] A user can delete individual tasks or bulk-clear completed ones
- [ ] Stats and progress bar update in real time
- [ ] Category tabs filter correctly
- [ ] Dark mode toggles and persists after page refresh
- [ ] All tasks persist after page refresh (localStorage)
- [ ] Error messages appear for invalid inputs
- [ ] App is usable on mobile (single column, touch-friendly)

---

## 10. Test Cases

### Typical Scenarios
| # | Action | Expected Result |
|---|---|---|
| T1 | Add task with all fields filled | Card appears with correct data |
| T2 | Add task with only title | Card appears without notes/deadline |
| T3 | Mark task as done | Card fades, stats update |
| T4 | Undo completed task | Card returns to active state |
| T5 | Edit task title and save | Card shows updated title |
| T6 | Delete a task | Card removed, stats update |
| T7 | Toggle dark mode | Theme switches, persists on reload |
| T8 | Filter by category | Only matching cards shown |
| T9 | Add deadline in the past | Card shows ⚠️ overdue indicator |
| T10 | Refresh page | All tasks still present |

### Edge Cases
| # | Action | Expected Result |
|---|---|---|
| E1 | Submit empty title | Error: "Task title cannot be empty." |
| E2 | Type 101 characters in title | Error: "Title is too long (max 100 characters)." |
| E3 | Type 501 characters in notes | Error: "Notes are too long (max 500 characters)." |
| E4 | Add 50+ tasks | All cards render, grid handles overflow |
| E5 | Very long single word in title | Card wraps text correctly (word-break) |
| E6 | Clear all completed when none exist | Nothing happens, no error |
| E7 | Edit and cancel | Original text restored, no changes saved |

---

## 11. Conclusions & Recommendations

1. **No backend needed** for the current scope — localStorage is sufficient
2. **Vanilla JS** is the right choice — keeps it simple, no build tools
3. **Single HTML file** makes deployment trivial (GitHub Pages, Netlify, or even email)
4. **Future priorities** if the app grows: add search, drag-and-drop reorder, and optional cloud sync (e.g. Firebase)
