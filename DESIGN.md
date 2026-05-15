---
version: alpha
name: ERP UI Blueprint
description: Compact light-mode ERP interface system built on shadcn/ui primitives.
colors:
  primary: "#171717"
  foreground: "#111827"
  secondary: "#374151"
  muted: "#4B5563"
  surface: "#FFFFFF"
  panel: "#F5F6F8"
  popover: "#FFFFFF"
  border: "#D8DDE6"
  input: "#DDE3EA"
  destructive: "#B91C1C"
  success: "#047857"
  warning: "#B45309"
  info: "#2563EB"
typography:
  heading-md:
    fontFamily: Sarabun
    fontSize: 16px
    fontWeight: 600
    lineHeight: 1.35
    letterSpacing: 0em
  body-md:
    fontFamily: Sarabun
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: 0em
  label-sm:
    fontFamily: Sarabun
    fontSize: 12px
    fontWeight: 500
    lineHeight: 1.25
    letterSpacing: 0.08em
rounded:
  sm: 4px
  md: 8px
  lg: 10px
  full: 9999px
spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 20px
  xl: 24px
  navbar-height: 56px
  sidebar-row: 32px
  sidebar-sub-row: 28px
components:
  app-shell:
    backgroundColor: "{colors.panel}"
    textColor: "{colors.foreground}"
    typography: "{typography.body-md}"
  card:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: "{spacing.lg}"
  sidebar:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.foreground}"
    width: 256px
  sidebar-mobile-close:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    size: 40px
  popover:
    backgroundColor: "{colors.popover}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: "{spacing.sm}"
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.surface}"
    typography: "{typography.body-md}"
    rounded: "{rounded.md}"
    height: 32px
    padding: "{spacing.md}"
  button-secondary:
    backgroundColor: "{colors.secondary}"
    textColor: "{colors.surface}"
    rounded: "{rounded.md}"
    height: 32px
  button-destructive:
    backgroundColor: "{colors.destructive}"
    textColor: "{colors.surface}"
    rounded: "{rounded.md}"
    height: 32px
  input:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    height: 32px
  table-header:
    backgroundColor: "{colors.panel}"
    textColor: "{colors.muted}"
    typography: "{typography.label-sm}"
  badge-success:
    backgroundColor: "{colors.success}"
    textColor: "{colors.surface}"
    rounded: "{rounded.full}"
  badge-warning:
    backgroundColor: "{colors.warning}"
    textColor: "{colors.surface}"
    rounded: "{rounded.full}"
  badge-info:
    backgroundColor: "{colors.info}"
    textColor: "{colors.surface}"
    rounded: "{rounded.full}"
  border-system:
    backgroundColor: "{colors.border}"
    height: 1px
  field-border:
    backgroundColor: "{colors.input}"
    height: 1px
---

# ERP UI Blueprint

## Overview

This design system describes a reusable ERP application shell for operational business software. It should feel compact, structured, fast, readable, and business-first. It should not feel promotional, editorial, decorative, dark, oversized, or elevated by visual effects.

The default source of truth for UI components is `shadcn/ui`. If `shadcn/ui` has a suitable primitive or pattern, shipped UI must use it for navigation, buttons, inputs, selects, cards, dialogs, tables, menus, popovers, badges, pagination, calendars, and other reusable building blocks. Compose ERP-specific patterns on top of those primitives instead of replacing them with browser-native controls or a second component system.

Project-specific data is replaceable. Brand names, menu labels, routes, dates, user identities, table rows, and page content are examples, not fixed product data. The shell structure, token choices, interaction rules, and component behavior are the stable parts.

Every clickable component must advertise interactivity with `cursor-pointer`. This applies to buttons, icon buttons, menu triggers, sidebar rows, pagination controls, tabs, dropdown triggers, list items that act like commands, and any custom surface that handles click or tap.

## Colors

The palette is light-mode only and built from neutral working surfaces with strong but quiet borders.

- **Primary (#171717):** Main action color, active shell affordances, and high-emphasis controls.
- **Foreground (#111827):** Default readable text for business content.
- **Secondary (#374151):** Secondary action fills when a filled control is needed.
- **Muted (#4B5563):** Metadata, helper text, table headers, and secondary labels.
- **Surface (#FFFFFF):** Cards, sidebar, navbar, popovers, dialogs, forms, and table surfaces.
- **Panel (#F5F6F8):** Application background and soft table-header background.
- **Border (#D8DDE6):** The visible structure of the ERP UI.
- **Input (#DDE3EA):** Form and field border treatment.
- **Destructive (#B91C1C):** Destructive actions and rejected states.
- **Success (#047857), Warning (#B45309), Info (#2563EB):** Status semantics for badges, tables, cards, details, and dashboard widgets.

Status mapping must remain consistent across the product: `Draft` is neutral, `Pending` is warning, `In Progress` is info, `Approved` and `Complete` are success, `Hold` is warning, and `Cancelled` or `Rejected` are destructive.

## Typography

Use `Sarabun` for both Thai and English. The base application size is `14px`, with compact metadata and controlled heading scale. Headings use medium to semibold weight; body text uses regular weight; labels use compact uppercase metadata styling only where scanning benefits from it, such as table headers.

Do not scale font size with viewport width. Keep letter spacing at `0` for normal text; reserve small positive letter spacing only for compact uppercase metadata labels.

## Layout

Every standard ERP page is made from three stable areas: `Navbar`, `Sidebar`, and `Content`. Reuse that structure even when business content changes.

```tsx
<SidebarProvider defaultOpen>
  <AppSidebar items={menuItems} footerItems={footerItems} brand={brand} />
  <SidebarInset className="min-h-screen bg-[#f5f6f8] text-slate-900">
    <ErpShellHeader
      userMenuItems={userMenuItems}
      userLabel={userLabel}
      userInitials={userInitials}
    />
    <div className="flex-1 px-4 py-4 md:px-5">{children}</div>
  </SidebarInset>
</SidebarProvider>
```

Core measurements:

- Navbar height: `h-14`
- Sidebar header height: `h-14`
- Navbar horizontal padding: `px-4 md:px-5`
- Content padding: `px-4 py-4 md:px-5`
- Sidebar content padding: `px-2 py-2`
- Top-level sidebar row height: `h-8`
- Sidebar child row height: `h-7`
- Utility control height: `h-8`
- Mobile sidebar close button size: `size-10`
- User avatar size: `size-6`
- Brand icon size: `size-8`

The navbar contains a left cluster for the sidebar trigger and a right cluster for the user menu. On mobile, the sidebar trigger must remain visible while the sidebar is closed so users can open navigation. Do not place a global search box, page title, page subtitle, calendar, date picker, or oversized call to action in the default navbar.

The sidebar uses full off-canvas collapse. It must hide completely when collapsed, stay locked to the viewport while content scrolls, support direct links and expandable parents, support nested child items, and use route-aware active states. When the sidebar is open on mobile, it must render its own close button inside the sidebar header because the navbar trigger may be covered by the off-canvas panel.

Content uses white cards on the neutral page background. Cards are the default grouping unit when a section has its own title, represents a workflow, groups related information, or needs a clear business boundary.

## Elevation & Depth

This system is intentionally flat. Do not use drop shadows or `box-shadow` in shipped ERP UI. Hierarchy comes from borders, white surfaces, neutral background bands, table header fills, typography weight, and status color.

Card headers should separate title metadata from body content with a horizontal border. Popovers, dropdowns, dialogs, and menus should use `shadcn/ui` surfaces with white fill, soft border, default radius, and custom hover or active states.

## Shapes

Use the default `shadcn` radius with compact enterprise geometry. Cards, inputs, buttons, menu triggers, and popovers should feel precise and operational, with enough rounding to be modern but not pill-like unless the component is a badge or avatar.

Use `rounded-md` as the default rectangular control shape, `rounded-lg` only where a larger repeated item needs a little more containment, and `rounded-full` for badges or circular controls only.

## Components

### Data Contracts

Drive the shell from data instead of hardcoded product labels.

```ts
type SidebarChildItem = {
  title: string;
  href: string;
  icon?: React.ComponentType<{ className?: string }>;
};

type SidebarItem = {
  title: string;
  href?: string;
  icon?: React.ComponentType<{ className?: string }>;
  badge?: string;
  defaultOpen?: boolean;
  children?: SidebarChildItem[];
};

type UserMenuItem = {
  label: string;
  onSelect?: () => void;
  href?: string;
};

type BrandConfig = {
  title: string;
  subtitle?: string;
  icon: React.ReactNode;
};
```

### Navbar

The sidebar trigger toggles the sidebar on every standard shell page. Use state-aware panel icons: `panel-left-close` when open and `panel-left-open` when closed. The user menu opens a dropdown aligned to the end and contains account-related or shell-level actions.

All navbar controls are compact and clickable elements use `cursor-pointer`. The navbar background stays white, its bottom border aligns with the sidebar header border, and it must not visually merge into the content area. On mobile, never hide the navbar sidebar trigger while the sidebar is closed.

### Sidebar

Use `collapsible="offcanvas"` and keep the sidebar fixed to the viewport:

```tsx
<Sidebar className="fixed inset-y-0 left-0 z-40 border-r border-[#d8dde6] bg-white text-slate-900">
  <SidebarHeader className="h-14 shrink-0 border-b border-[#d8dde6] bg-white px-4 py-0">
    <div className="flex w-full items-center justify-between gap-3">
      <BrandLockup brand={brand} />
      <SidebarTrigger className="size-10 shrink-0 cursor-pointer rounded-md border border-[#d8dde6] bg-white text-slate-900 hover:bg-slate-100 md:hidden">
        <PanelLeftCloseIcon className="size-5" />
      </SidebarTrigger>
    </div>
  </SidebarHeader>
  <SidebarContent className="min-h-0 flex-1 overflow-y-auto bg-white px-2 py-2" />
  <SidebarFooter className="shrink-0 border-t border-[#d8dde6] bg-white p-2" />
</Sidebar>
```

Mobile sidebar close behavior is required:

- Render a visible `SidebarTrigger` or equivalent close button inside the sidebar header on mobile.
- Place it on the right side of the brand lockup so it is visible in the first viewport.
- Use `PanelLeftCloseIcon` or an equivalent close/collapse icon.
- Keep the button at least `size-10` with `shrink-0`, `cursor-pointer`, border, and white background.
- Do not rely only on the navbar trigger to close the mobile sidebar because the open sidebar can cover that trigger.
- Do not hide the close button behind the brand block, sidebar content scroll area, or overlay layer.

Direct-link items navigate to their route and follow route-aware active state. Expandable parent items toggle local open state. Child rows navigate to routes and remain visually subordinate through indentation, not smaller typography.

### Cards

Use `shadcn/ui` card primitives with white fill, soft border, and no shadow.

```tsx
<Card className="border-[#d8dde6] bg-white shadow-none">
  <CardHeader className="space-y-1 border-b border-[#d8dde6] px-5 py-4">
    <CardTitle className="text-base font-semibold text-slate-900">
      Section title
    </CardTitle>
    <CardDescription className="text-sm text-slate-500">
      Short supporting description.
    </CardDescription>
  </CardHeader>
  <CardContent className="space-y-4 px-5 py-4">{content}</CardContent>
</Card>
```

### Buttons And Menus

Buttons come from `shadcn/ui`, use compact spacing, and use `cursor-pointer`. Primary buttons use `--primary`; destructive buttons use `--destructive`; secondary, outline, and ghost variants should stay visually quieter than the main action.

Menus, dropdowns, context menus, menubars, popovers, command pickers, date pickers, date ranges, and calendar popovers must render as the default `shadcn/ui` surfaces. Do not restyle them back toward browser-native chrome, and do not replace them with browser-default dropdown, context menu, calendar, or date chooser UI.

For dropdown and menu-list behavior, `shadcn/ui` is mandatory:

- Use the default `shadcn/ui` primitive for `DropdownMenu`, `Menubar`, `ContextMenu`, `Command`, `Select`, `Popover`, and related menu-list surfaces.
- Keep the shipped interaction model, layering, spacing, and keyboard behavior aligned with `shadcn/ui` defaults unless a project-specific extension is required.
- Do not use native `<select>` or browser-rendered option lists as the final shipped UI.
- Do not use browser-native context menus or browser-native date or calendar pickers as the final shipped UI.

### Forms

Forms are compact, structured, and operational. Required coverage includes text input, select, textarea, checkbox, radio group, switch, disabled field, read-only field, file upload, thumbnail image preview with remove action, date picker, and date range.

Select-like controls must use `shadcn/ui` `Select`, not native `<select>` as the shipped UI. Date controls must use a `shadcn/ui` popover/calendar pattern, not native `<input type="date">`, `<input type="datetime-local">`, `<input type="month">`, or `<input type="week">`.

Any form or list component that opens choices, commands, or navigation must use `cursor-pointer` on the interactive trigger and interactive items.

Image upload previews are small thumbnails with a corner remove action. Removing one image removes only that preview item, and previews stay compact enough to support repeated business workflows.

### Tables

Tables are a primary ERP surface. They require clear headers, compact rows, status badges, gray table header background, horizontal overflow handling, rows-per-page selector, and pagination.

Wrap wide tables in an `overflow-x-auto` container and give the table a minimum width so columns remain readable. Pagination and rows-per-page controls live in the same footer area. Rows-per-page options are `10`, `20`, `50`, and `100`.

## Do's and Don'ts

- Do use `shadcn/ui` primitives or patterns whenever they exist.
- Do keep the shell data-driven and replace only project-specific brand, menu, route, user, and content data.
- Do keep navbar, sidebar, and content structure consistent across standard ERP pages.
- Do render a visible mobile close/collapse button inside the open sidebar header.
- Do use borders and flat surfaces for hierarchy.
- Do keep all clickable controls visibly interactive with `cursor-pointer`.
- Do use the default `shadcn/ui` primitives for dropdowns and menu lists.
- Do keep table status colors consistent with the shared status mapping.
- Don't rely only on a navbar trigger to close an open mobile sidebar.
- Don't ship clickable components without `cursor-pointer`.
- Don't use browser-native select, date, calendar, dropdown, or context-menu UI in shipped screens.
- Don't use drop shadow or `box-shadow` in any shipped UI state.
- Don't add default navbar search, page titles, date controls, or oversized calls to action without a clear shell-level reason.
- Don't reduce sidebar child typography below the top-level menu size.
- Don't let wide tables break the page; contain horizontal scroll inside the table region.
- Don't hardcode one project's module tree into reusable shell logic.
