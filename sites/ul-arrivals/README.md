# ul-arrivals

The site itself: one self-contained `index.html`.

No build step and no dependencies. Everything — markup, styles, data and logic — lives in that file. The only external requests are Google Fonts and, when a visitor chooses to sign in, the Appwrite Web SDK from jsDelivr.

## Editing the content

The data lives in plain arrays near the top of the script block.

**`GROUPS`** — the checklist. Each item:

```js
{
  id: 'stable-slug',        // never reuse or rename — it keys saved ticks
  p: 'now'|'fly'|'ie',      // phase chip: Do now / Before you fly / In Ireland
  t: 'Title',
  d: 'Detail shown underneath',
  only: { visa:'waiting'|'approved', stay:'sorted'|'looking' }  // optional
}
```

`only` is what makes the list adapt. Omit it and the item always shows. An item with `only:{stay:'looking'}` appears solely for people still hunting for accommodation.

**`EVENTS`** — the calendar and timeline, sharing one source:

```js
{ d:'2026-09-07', t:'Title', x:'Detail', k:'key'|'warn'|'orient'|'pay'|'trip'|'', tag:'Academic' }
```

`k` drives the dot colour and cell tint; `tag` is the chip in the tooltip and agenda.

The calendar grid is generated from `Date`, so weekday alignment is always correct — change the year and it still lines up.

## Changing item IDs

Don't, unless you mean to. Ticks are stored as `{ id: true }` in `localStorage` and in the synced row. Renaming an `id` silently unticks it for everyone who had it checked.

## Accessibility

Checklist rows are `role="checkbox"` with `tabindex`, toggled by Space or Enter. Calendar cells with events are focusable and open the same tooltip on focus as on hover, dismissible with Escape. The page prints — each view starts on its own page.
