---
name: header-submenu-flyout
description: How the header1 sub-menu (menu-sub block) nested flyouts work and what hid them
metadata:
  type: project
---

The `header1` layout's "Menu sub" block renders nested navigation via [[snippets/menu-sub.liquid]], which reads children from Shopify's native `link.links` (with a fallback to a linklist named after the link handle). The CSS that styles 3 levels (`.sub-menu` > `supmenu-dropdown` > `childsupmenu-dropdown`) lives in `assets/style.css`, but `header1`-prefixed overrides inside `sections/header.liquid` (the `@media (min-width:1199px)` block near line 4871) win on specificity.

**Why nested flyouts were invisible:** that header.liquid block set `overflow: hidden` on `.sub-menu`/`supmenu-dropdown` (for rounded corners), which clipped the `left:100%` nested flyouts; and its hover rules set only `transform`, not `display/opacity/visibility`. Fix: `overflow: visible` on the panels, and hover rules that explicitly set `display:block; opacity:1; visibility:visible` and include `.collapse:not(.show)` to beat Bootstrap's collapse class.

**Why:** edits appeared to "do nothing" — the dropdowns carry Bootstrap's `collapse` class (for the mobile accordion), and the more-specific header.liquid rules quietly overrode style.css.
**How to apply:** when touching sub-menu visibility, edit the `.header1`-prefixed rules in `sections/header.liquid`, not just `assets/style.css`.
