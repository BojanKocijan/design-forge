# Component Patterns — Design Forge

**Version:** 1.0.0
**Last Updated:** 2026-06-06
**Binding:** Yes — these patterns represent validated, reusable solutions established across projects. Apply them before building from scratch.

---

## 1. ResponsiveModal

Forms and creation dialogs should adapt to breakpoint rather than always using a bottom sheet.

```
Desktop (≥1024px) → centred Dialog overlay (Radix Dialog)
Mobile/tablet     → bottom Sheet (Radix Sheet, slides from bottom)
```

**Implementation:**
```tsx
// src/components/shared/ResponsiveModal.tsx
import { useIsDesktop } from '@/hooks/useIsDesktop'
import { Dialog, DialogContent, DialogHeader, DialogTitle } from '@/components/ui/dialog'
import { Sheet, SheetContent } from '@/components/ui/sheet'

export function ResponsiveModal({ open, onClose, title, children }) {
  const isDesktop = useIsDesktop() // default 1024px breakpoint

  if (isDesktop) {
    return (
      <Dialog open={open} onOpenChange={onClose}>
        <DialogContent className="max-w-lg rounded-2xl p-0 gap-0">
          <DialogHeader className="px-6 pt-6 pb-0">
            <DialogTitle>{title}</DialogTitle>
          </DialogHeader>
          <div className="px-6 pb-6 pt-5">{children}</div>
        </DialogContent>
      </Dialog>
    )
  }

  return (
    <Sheet open={open} onOpenChange={onClose}>
      <SheetContent side="bottom" className="rounded-t-2xl p-0" hideClose>
        {/* handle + title + X + children */}
      </SheetContent>
    </Sheet>
  )
}
```

**useIsDesktop hook:**
```ts
// src/hooks/useIsDesktop.ts
import { useEffect, useState } from 'react'

export function useIsDesktop(breakpoint = 1024) {
  const [isDesktop, setIsDesktop] = useState(() => window.innerWidth >= breakpoint)
  useEffect(() => {
    const mq = window.matchMedia(`(min-width: ${breakpoint}px)`)
    const handler = (e: MediaQueryListEvent) => setIsDesktop(e.matches)
    mq.addEventListener('change', handler)
    return () => mq.removeEventListener('change', handler)
  }, [breakpoint])
  return isDesktop
}
```

---

## 2. Speed-dial FAB (mobile-only)

On mobile apps, replace a single FAB with a speed dial that expands upward. Actions are ordered bottom→top by thumb priority (most important action closest to thumb).

**Pattern:**
```
[New Project] ← disabled/coming soon, furthest from thumb
[New Client]
[Send Invoice] ← primary, closest to thumb
[+] FAB toggle ← bottom-right fixed
```

**Rules:**
- Always `lg:hidden` — desktop navigation handles these actions differently
- FAB stacks with flex-col, actions above the toggle button
- Backdrop div closes dial on outside tap
- Primary action gets `bg-primary`, secondary actions get `bg-white border`
- Disabled items get `opacity-50 cursor-not-allowed`
- Desktop FAB: decide per-project — often hidden in favour of sidebar + header buttons

---

## 3. Breakpoint-aware view toggle

When a page has both a card/grid view and a table/list view:
- **Mobile (<640px):** Always show cards. Hide the toggle entirely. Tables are not usable on small screens.
- **Tablet/desktop (≥640px):** Show toggle. Default to table/list for data density.

```tsx
const isDesktop = useIsDesktop(640) // sm breakpoint
const effectiveView: ViewMode = isDesktop ? view : 'grid'

// In JSX:
{isDesktop && <ViewToggle mode={view} onChange={setView} />}
```

---

## 4. Underline tabs with counts

Filter chips are the wrong pattern for list pages. Use underline-style tabs with count badges.

```
All Clients 20  |  Owing 5       ← Clients
All Invoice 20  |  Open 5  |  Past Due 3  |  Paid 12  ← Invoices
```

**Rules:**
- Never pill/chip filter buttons for primary list filtering
- Count badge sits next to the label, same colour as active/inactive text
- "New" tab (recency-based) is usually useless — prefer action-oriented tabs like "Owing"

---

## 5. Data tables

When displaying structured data (invoices, clients):
- Use a proper `<table>` element with column headers (ALL CAPS, muted, tracked)
- Table is horizontally scrollable (`overflow-x-auto`, `min-w-[580px]`)
- Progressive column reveal: critical columns always visible, secondary hidden at `sm:`/`md:`
- Row hover: `hover:bg-muted/40`, full row tappable → detail sheet
- Actions column: hover-reveal on desktop (`opacity-0 group-hover:opacity-100`), always visible on mobile

---

## 6. Card primary action

On entity cards (clients, projects), the primary CTA should be the **business action**, not navigation.

```
❌ "See details ›"  ← navigation, secondary
✓ "Send Invoice"    ← business action, primary
```

- Full card tap → detail/info sheet
- Primary button → the action the user is most likely to want
- 3-dot menu: `opacity-0 group-hover:opacity-100`, white bg + border + shadow + z-10

---

## 7. Live avatar preview in forms

When creating a contact/client/person, show a live colour-coded avatar that updates as the name is typed.

```tsx
const initials = name.trim().split(' ').map(w => w[0]).join('').toUpperCase().slice(0, 2)
const color = name.trim() ? getAvatarColor(name) : '#e5e7eb'

<div className="w-16 h-16 rounded-full flex items-center justify-center text-xl font-bold text-white"
     style={{ background: color }}>
  {initials || <span className="text-white/50 text-sm">?</span>}
</div>
```

Avatar preview is always text/initials until image upload is explicitly added.

---

## 8. Form field layout in modals

Two-column grid for related pairs (email/phone, company/address). Full-width for primary fields (name).

```
Name *                    (full width)
Email       |    Phone    (two columns)
Company     |    Address  (two columns)
            [Cancel] [Create]  (right-aligned)
```

Labels: ALL CAPS, `text-[12px] font-semibold text-muted-foreground uppercase tracking-wide`

---

## 9. No emojis in UI

Never use emoji characters in interactive UI elements, button labels, status text, or any user-facing copy.

Use Lucide icons or plain text instead:
- `💰` → `<CreditCard className="w-4 h-4" />`
- `✓` → `<Check className="w-4 h-4" />`
- `⚠` → `<AlertTriangle className="w-4 h-4" />`
- `→` → `<ChevronRight className="w-4 h-4" />`

Only add emojis if the user explicitly requests them.

---

## 10. Touch target sizing (mobile-first apps)

For apps used on-site by contractors, tradespeople, or anyone who may be using a phone with one hand:

| Element | Minimum size |
|---|---|
| Inputs | h-12 (48px) |
| Primary buttons | h-12, py-3 |
| List rows | min 64px height |
| Avatar (cards) | 48px |
| Avatar (list rows) | 32-40px |
| Page padding | px-5 (20px) |

---

## 11. 3-dot context menu on cards

Entity cards (clients, projects) should have a context menu accessible via a 3-dot button (MoreHorizontal icon).

### Device-aware visibility
```tsx
// Always visible on touch, hover-reveal on mouse — CSS only, no JS
'[@media(hover:none)]:opacity-100',
'[@media(hover:hover)]:opacity-0 [@media(hover:hover)]:group-hover:opacity-100',
```

### No event conflict rule
**Critical:** The 3-dot trigger must be a SIBLING to the clickable area, NOT a child of it. If the whole card is clickable, the trigger must sit outside that clickable div.

```tsx
// ✅ Correct — 3-dot is sibling, no conflict
<div className="flex items-start gap-2">
  <div onClick={onDetails} className="flex-1">  ← clickable area
    <Avatar /> <Name />
  </div>
  <DropdownMenu>                                 ← sibling, no conflict
    <DropdownMenuTrigger className="... [@media(hover:none)]:opacity-100 ...">
      <MoreHorizontal />
    </DropdownMenuTrigger>
  </DropdownMenu>
</div>

// ❌ Wrong — 3-dot inside clickable div, stopPropagation needed but brittle
<div onClick={onDetails}>
  <Avatar /> <Name />
  <DropdownMenuTrigger onClick={e => e.stopPropagation()}>  ← fragile
```

### Standard actions for client cards
1. **Send Invoice** — opens wizard with client pre-selected
2. **Edit Client** — opens create form pre-filled, "Update" button
3. **View Details** — opens detail sheet
4. **Remove Client** — shows confirmation dialog (soft delete)

### Soft delete pattern
Always soft-delete rather than hard-delete for entities that have related records (invoices, projects).
```ts
// Storage
removeClient: (id) => {
  lsSet(KEY, lsGet(KEY).map(c => c.id === id ? { ...c, deleted_at: new Date().toISOString() } : c))
}
getClients: () => lsGet(KEY).filter(c => !c.deleted_at)
```

### Confirmation dialog
Always confirm destructive actions via a `ResponsiveModal`:
```tsx
<ResponsiveModal open title="Remove client" onClose={() => setTarget(null)}>
  <p>Remove <b>{target.name}</b>? Their invoices will not be deleted.</p>
  <div className="flex justify-end gap-3 pt-4">
    <button onClick={() => setTarget(null)}>Cancel</button>
    <button onClick={handleConfirm} className="... bg-destructive">Remove</button>
  </div>
</ResponsiveModal>
```

---

## Changelog

- **1.0.0 (2026-06-06)** — Initial version. Patterns extracted from ReMoDo project (invoicing SaaS for Dutch contractors). Covers: ResponsiveModal, speed-dial FAB, breakpoint view toggle, underline tabs, data tables, card primary action, live avatar, form layout, no-emoji rule, mobile touch sizing.
