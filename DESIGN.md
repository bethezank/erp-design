# ERP UI Blueprint

This document is a reusable blueprint for building ERP interfaces with a consistent shell, consistent interactions, and consistent component behavior.

It is intentionally project-agnostic.
Do not treat any label, menu name, route, date, or user identity in examples as fixed product data.
Treat them as replaceable placeholders inside a stable UI system.

## Goal

This blueprint exists so a team can start a new ERP project and reproduce the same product quality repeatedly without redesigning the shell each time.

The blueprint should guarantee consistency in:

- layout
- spacing
- borders
- navigation hierarchy
- form behavior
- table behavior
- status colors
- interaction patterns

## Component Source Rule

The default source of truth for UI components is `shadcn/ui`.

This is a non-negotiable rule for shipped UI.
If `shadcn/ui` has a suitable default primitive or pattern, it must be used.
This applies to every component in the product shell and every business screen.
Treat `shadcn/ui` as mandatory for navbar, sidebar, buttons, inputs, selects, cards, dialogs, tables, menus, popovers, badges, pagination, and all other reusable UI building blocks.
Do not ship browser-native form UI in its place.

When building a new project from this blueprint:

1. install the shadcn skill first
2. use `shadcn/ui` components as the primary building blocks
3. compose custom ERP patterns on top of those primitives
4. create custom components only when `shadcn/ui` does not already provide a suitable base

Never replace an available `shadcn/ui` component with a custom or browser-native alternative just for convenience.

Install command:

```bash
npx skills add shadcn/ui
```

This rule exists to keep future projects:

- consistent
- fast to scaffold
- easy to maintain
- easy to extend without inventing a second component system

## Non-Native Control Rule

This is a hard rule.
Shipped controls must render with `shadcn/ui` surfaces and interactions, not browser UI.

Dropdown-like controls must use `shadcn/ui` components, not browser-native UI.

This applies to:

- select
- dropdown menu
- context menu
- menubar
- popover-based action menus
- command-driven option pickers
- date picker
- date range picker
- calendar popovers

Do not use:

- native browser `<select>` styling as the final UI
- native browser `<input type="date">` as the final UI
- native browser `<input type="datetime-local">` as the final UI
- native browser `<input type="month">` as the final UI
- native browser `<input type="week">` as the final UI
- browser calendar popovers
- browser time/date chooser UI
- browser-default context menus
- unstyled HTML dropdown surfaces

The visual target is the default `shadcn/ui` surface style:

- white floating panel
- soft border
- default `shadcn` radius
- custom hover state
- custom active state
- consistent typography

This rule is mandatory because browser-native dropdown UIs are inconsistent across operating systems and break the design system.

## Design Direction

- Light mode only
- Compact enterprise layout
- Default `shadcn` radius
- Sarabun for both Thai and English
- Base application font size: `14px`
- Strong border structure
- White working surfaces on a soft neutral page background
- Functional and operational tone
- Minimal decoration
- No drop shadow anywhere in the shipped ERP UI

The product should feel:

- structured
- fast
- readable
- business-first

The product should not feel:

- promotional
- editorial
- overly rounded
- dark
- decorative
- elevated by shadow effects

## Theme Tokens

Use these as the default shell tokens.

```css
:root {
  --background: #f5f6f8;
  --foreground: #111827;
  --card: #ffffff;
  --card-foreground: #111827;
  --popover: #ffffff;
  --popover-foreground: #111827;
  --primary: #171717;
  --primary-foreground: #ffffff;
  --secondary: #171717;
  --secondary-foreground: #ffffff;
  --muted: #eef0f3;
  --muted-foreground: #6b7280;
  --accent: #171717;
  --accent-foreground: #ffffff;
  --destructive: #b91c1c;
  --border: #d8dde6;
  --input: #dde3ea;
  --ring: rgba(23, 23, 23, 0.18);
  --sidebar: #ffffff;
  --sidebar-foreground: #111827;
  --sidebar-primary: #171717;
  --sidebar-primary-foreground: #ffffff;
  --sidebar-accent: #f3f4f6;
  --sidebar-accent-foreground: #111827;
  --sidebar-border: #d8dde6;
  --sidebar-ring: rgba(23, 23, 23, 0.18);
  --radius: 0.625rem;
}
```

## Typography

- Primary font: `Sarabun`
- Heading weight: medium to semibold
- Body weight: regular
- Metadata: compact and low-emphasis
- Large numeric values: prominent but controlled

## Shell Principles

Every standard ERP page should be made from three stable areas:

1. Navbar
2. Sidebar
3. Content

These three areas define the product shell.
They should be reused exactly in structure, even when business content changes.

## Main Layout Blueprint

This is the default composition for every standard ERP page.

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

## Shared Measurements

Use these as the default shell measurements.

- Navbar height: `h-14`
- Sidebar header height: `h-14`
- Navbar horizontal padding: `px-4 md:px-5`
- Content padding: `px-4 py-4 md:px-5`
- Sidebar content padding: `px-2 py-2`
- Base application font size: `14px`
- Sidebar top-level row height: `h-8`
- Sidebar sub-menu row height: `h-7`
- Sidebar menu and sub-menu font size: `14px`; identical for both levels
- Utility control height: `h-8`
- User avatar size: `size-6`
- Brand icon size: `size-8`

## Data Contracts

The shell should be driven by data instead of hardcoded product-specific labels.

### Sidebar Menu Shape

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
```

### User Menu Shape

```ts
type UserMenuItem = {
  label: string;
  onSelect?: () => void;
  href?: string;
};
```

### Brand Shape

```ts
type BrandConfig = {
  title: string;
  subtitle?: string;
  icon: React.ReactNode;
};
```

Recommended default for this shell:

```ts
const brand = {
  title: "Bethezank Lab",
  subtitle: "www.bethezank.com",
};
```

These shapes are intentionally generic so the shell can be reused in a new project without changing the underlying UI rules.

## Navbar

The navbar is the fixed top action bar.

### Navbar Purpose

- expose sidebar toggle
- expose user account actions
- remain visually stable across all pages

### Navbar Structure

The navbar should always render two clusters:

1. left cluster
2. right cluster

The left cluster should contain:

- sidebar trigger

The right cluster should contain:

- user menu trigger

### Navbar Rules

- No global search box by default
- No page title in the navbar by default
- No page subtitle in the navbar by default
- No calendar or date control in the navbar
- No oversized CTA in the navbar
- All navbar controls must remain compact
- All clickable navbar elements must use `cursor-pointer`
- The sidebar trigger should use the same panel-style icon shown in the reference layout

### Navbar Wrapper

```tsx
export function ErpShellHeader({
  userLabel,
  userInitials,
  userMenuItems,
}: {
  userLabel: string;
  userInitials: string;
  userMenuItems: UserMenuItem[];
}) {
  return (
    <header className="sticky top-0 z-30 h-14 border-b border-[#d8dde6] bg-white">
      <div className="flex h-full items-center justify-between px-4 md:px-5">
        <div className="flex items-center gap-2">
          <SidebarTrigger className="cursor-pointer rounded-md border border-[#d8dde6] bg-white text-slate-900 hover:bg-slate-100" />
        </div>

        <div className="flex items-center gap-2">
          <DropdownMenu>
            <DropdownMenuTrigger className="inline-flex h-8 cursor-pointer items-center gap-2 rounded-md border border-[#d8dde6] bg-white px-2 text-left outline-none transition hover:bg-slate-100">
              <Avatar className="size-6 rounded-md border border-[#d8dde6]">
                <AvatarFallback className="bg-[#171717] text-[11px] text-white">
                  {userInitials}
                </AvatarFallback>
              </Avatar>
              <span className="hidden text-sm font-medium text-slate-900 sm:block">
                {userLabel}
              </span>
              <ChevronDownIcon className="size-4 text-slate-500" />
            </DropdownMenuTrigger>

            <DropdownMenuContent align="end">
              {userMenuItems.map((item) => (
                <DropdownMenuItem key={item.label}>{item.label}</DropdownMenuItem>
              ))}
            </DropdownMenuContent>
          </DropdownMenu>
        </div>
      </div>
    </header>
  );
}
```

### Navbar Events

#### Sidebar Trigger

- toggles sidebar open and closed
- must work on every standard shell page
- must use state-aware panel icons
- when the sidebar is currently open, use `panel-left-close`
- when the sidebar is currently closed, use `panel-left-open`

Recommended trigger content:

```tsx
<Button
  variant="outline"
  size="icon"
  className="size-10 cursor-pointer rounded-xl border-[#d8dde6] bg-white text-slate-900 shadow-none"
>
  {isSidebarOpen ? (
    <PanelLeftCloseIcon className="size-5" />
  ) : (
    <PanelLeftOpenIcon className="size-5" />
  )}
</Button>
```

#### Utility Actions

- are not part of the default navbar for this shell
- should be added only if the project later has a clear shell-level need
- must not reintroduce a calendar or date picker into the navbar

#### User Menu

- opens a dropdown aligned to the end
- contains account-related or shell-level actions

### Navbar Alignment Rules

- Navbar bottom border must align with the sidebar header bottom border
- Navbar background must stay white
- Navbar must not visually merge into the content area

## Sidebar

The sidebar is the persistent module navigation area.

### Sidebar Purpose

- define module hierarchy
- expose top-level navigation
- expose second-level navigation
- preserve user orientation across the app

### Sidebar Behavior Rules

- Must use full off-canvas collapse
- Must hide 100% when collapsed
- Must not use icon-only collapse as the default ERP behavior
- Must stay locked to the viewport while page content scrolls
- Must support direct-link items
- Must support expandable parent items
- Must support nested child items
- Must use route-aware active states
- All clickable rows must use `cursor-pointer`

### Sidebar Wrapper

```tsx
export function AppSidebar({
  items,
  footerItems,
  brand,
}: {
  items: SidebarItem[];
  footerItems?: SidebarChildItem[];
  brand: BrandConfig;
}) {
  const pathname = usePathname();
  const [openMenus, setOpenMenus] = useState<Record<string, boolean>>(
    Object.fromEntries(
      items
        .filter((item) => item.children?.length)
        .map((item) => [item.title, Boolean(item.defaultOpen)])
    )
  );

  const toggleMenu = (title: string) => {
    setOpenMenus((current) => ({
      ...current,
      [title]: !current[title],
    }));
  };

  return (
    <Sidebar
      collapsible="offcanvas"
      variant="sidebar"
      className="fixed inset-y-0 left-0 z-40 border-r border-[#d8dde6] bg-white text-slate-900"
    >
      <SidebarHeader className="h-14 justify-center border-b border-[#d8dde6] bg-white px-4 py-0">
        <div className="flex items-center gap-3">
          <div className="flex size-8 items-center justify-center rounded-md border border-[#d8dde6] bg-[#171717] text-white">
            {brand.icon}
          </div>
          <div>
            <p className="truncate text-sm font-semibold text-slate-900">
              {brand.title}
            </p>
            {brand.subtitle ? (
              <p className="truncate text-[11px] text-slate-500">
                {brand.subtitle}
              </p>
            ) : null}
          </div>
        </div>
      </SidebarHeader>

      <SidebarContent className="bg-white px-2 py-2">
        <SidebarGroup className="gap-1">
          <SidebarGroupLabel className="px-2 text-[11px] uppercase tracking-[0.18em] text-slate-500">
            Workspace
          </SidebarGroupLabel>

          <SidebarGroupContent>
            <SidebarMenu className="gap-1">
              {items.map((item) => (
                <SidebarMenuItem key={item.title}>
                  {item.children?.length ? (
                    <>
                      <SidebarMenuButton
                        size="default"
                        onClick={() => toggleMenu(item.title)}
                        className="h-8 px-2 text-sm"
                      >
                        {item.icon ? <item.icon className="size-4" /> : null}
                        <span>{item.title}</span>
                        {item.badge ? (
                          <SidebarMenuBadge className="right-7 top-1.5 min-w-4 px-1 text-[10px] text-slate-500">
                            {item.badge}
                          </SidebarMenuBadge>
                        ) : null}
                        <ChevronDownIcon
                          className={`ml-auto size-4 transition-transform ${
                            openMenus[item.title] ? "rotate-180" : ""
                          }`}
                        />
                      </SidebarMenuButton>

                      {openMenus[item.title] ? (
                        <SidebarMenuSub className="mx-2 mt-1 gap-1 border-l border-[#d8dde6] px-2 py-1">
                          {item.children.map((subItem) => (
                            <SidebarMenuSubItem key={subItem.title}>
                              <SidebarMenuSubButton
                                render={<Link href={subItem.href} />}
                                className="h-7 px-2 text-sm"
                              >
                                {subItem.icon ? (
                                  <subItem.icon className="size-3.5" />
                                ) : null}
                                <span>{subItem.title}</span>
                              </SidebarMenuSubButton>
                            </SidebarMenuSubItem>
                          ))}
                        </SidebarMenuSub>
                      ) : null}
                    </>
                  ) : (
                    <SidebarMenuButton
                      isActive={pathname === item.href}
                      render={<Link href={item.href ?? "#"} />}
                      className="h-8 px-2 text-sm"
                    >
                      {item.icon ? <item.icon className="size-4" /> : null}
                      <span>{item.title}</span>
                    </SidebarMenuButton>
                  )}
                </SidebarMenuItem>
              ))}
            </SidebarMenu>
          </SidebarGroupContent>
        </SidebarGroup>
      </SidebarContent>

      {footerItems?.length ? (
        <SidebarFooter className="border-t border-[#d8dde6] bg-white p-2">
          <SidebarMenu>
            {footerItems.map((item) => (
              <SidebarMenuItem key={item.title}>
                <SidebarMenuButton className="h-8 px-2 text-sm">
                  {item.icon ? <item.icon className="size-4" /> : null}
                  <span>{item.title}</span>
                </SidebarMenuButton>
              </SidebarMenuItem>
            ))}
          </SidebarMenu>
        </SidebarFooter>
      ) : null}

      <SidebarRail />
    </Sidebar>
  );
}
```

### Sidebar Event Model

#### Direct-Link Item

- click navigates to its route
- active state follows the current route

#### Expandable Parent Item

- click toggles local open state
- parent row is an interaction row by default
- it may optionally navigate in a different project, but that should be a deliberate rule

#### Child Item

- click navigates to its route
- child row must remain visually subordinate to the parent

#### Collapse Trigger

- controlled from the navbar sidebar trigger
- when collapsed, the sidebar leaves the content area completely

### Sidebar Visual Rules

- White background
- Shared border color
- Header height must match navbar height
- Sidebar shell must be viewport-locked
- Sidebar internal menu area may scroll only if the menu content exceeds viewport height
- Menu spacing must stay compact
- Top-level row height: `h-8`
- Child row height: `h-7`
- Sub-menu text size must match top-level menu text size
- Do not use reduced typography for child items
- Sub-menu indentation must stay visible
- Chevron rotation must communicate open state

### Sidebar Scroll Rule

This is a required shell behavior.

- The page scrollbar must scroll only the content area
- The sidebar must remain fixed in place
- The sidebar must not visually slide upward while the main page scrolls
- If a project has a very long menu, only the sidebar's internal content region should scroll

Recommended structure:

```tsx
<Sidebar className="fixed inset-y-0 left-0 z-40 border-r border-[#d8dde6] bg-white text-slate-900">
  <SidebarHeader className="h-14 shrink-0 border-b border-[#d8dde6] bg-white px-4 py-0" />
  <SidebarContent className="min-h-0 flex-1 overflow-y-auto bg-white px-2 py-2" />
  <SidebarFooter className="shrink-0 border-t border-[#d8dde6] bg-white p-2" />
</Sidebar>
```

### Sidebar Configuration Guidance

The exact menu tree should be configured per project.
Do not hardcode one project's modules into the blueprint.

Recommended structure:

- direct-link items for top-level pages
- expandable parent items for grouped modules
- child items for second-level flows
- optional footer utilities for settings or system tools

Recommended defaults:

- open one important section by default so hierarchy is visible
- keep the rest collapsed to reduce noise

## Content

The content area is the main work surface.

### Content Purpose

- host cards
- host forms
- host tables
- host detail panels
- host charts and workflow UI

### Content Wrapper

```tsx
<div className="flex-1 px-4 py-4 md:px-5">{children}</div>
```

### Content Rules

- Page background stays on the shared neutral background
- Main work areas use white cards
- Borders stay visible but soft
- Spacing stays compact
- Cards are the default grouping unit
- Card structure should be built from `shadcn/ui` card primitives
- Card surfaces must use real background fill, not shadow as a simulated background
- Decorative empty space should be minimized
- Card headers should visually separate title metadata from the card body

### Card Blueprint

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
  <CardContent className="space-y-4 px-5 py-4">
    {content}
  </CardContent>
</Card>
```

Use cards when:

- a section has its own title
- a section represents one workflow
- a section groups related information
- a section needs a clear business boundary

### Card Header Rule

This is the default ERP card pattern.

- Card header should contain a title and an optional subtitle
- Card header should be separated from the body with a horizontal border
- Title should use stronger contrast than the subtitle
- Subtitle should stay readable but lower-emphasis
- Body content should begin below the separator line, not visually merge into the header
- Cards must not use shadow as the background surface or as the primary separation treatment
- Prefer white fill plus border; keep card shadow removed with `shadow-none`

## Buttons

Buttons should come from `shadcn/ui` and follow compact enterprise spacing.

```tsx
<div className="flex flex-wrap gap-2">
  <Button className="cursor-pointer">Primary action</Button>
  <Button variant="secondary" className="cursor-pointer">
    Secondary
  </Button>
  <Button variant="outline" className="cursor-pointer">
    Outline
  </Button>
  <Button variant="ghost" className="cursor-pointer">
    Ghost
  </Button>
  <Button variant="destructive" className="cursor-pointer">
    Destructive
  </Button>
</div>
```

Rules:

- use compact spacing
- primary button color uses `--primary`
- all buttons must use `cursor-pointer`
- do not oversize buttons unless there is a true workflow reason

## Badges

Badges should cover both generic badges and ERP workflow status badges.

```tsx
<div className="flex flex-wrap gap-2">
  <Badge>Default</Badge>
  <Badge variant="secondary">Secondary</Badge>
  <Badge variant="outline">Outline</Badge>
  <Badge className="border border-emerald-200 bg-emerald-50 text-emerald-700">
    Approved
  </Badge>
  <Badge className="border border-amber-200 bg-amber-50 text-amber-700">
    Pending
  </Badge>
  <Badge className="border border-rose-200 bg-rose-50 text-rose-700">
    Cancelled
  </Badge>
</div>
```

### Shared Status Mapping

- Draft: neutral
- Pending: amber
- In Progress: blue
- Approved: success green
- Complete: success green
- Hold: orange
- Cancelled: rose
- Rejected: red

This mapping must stay identical across:

- badges
- tables
- cards
- detail screens
- dashboard widgets

## Forms

Forms should feel compact, structured, and operational.

### Required Form Coverage

- text input
- select
- textarea
- checkbox
- radio group
- switch
- disabled field
- read-only field
- file upload
- thumbnail image preview with remove action
- date picker
- date range

### Form Rules

- controls align cleanly in grid layouts
- controls use shared tokens
- controls avoid consumer-style oversized presentation
- all actionable controls use `cursor-pointer`
- select-like controls must use `shadcn/ui` `Select`, not browser-native `<select>` as the shipped UI
- date-related controls must use a `shadcn/ui` popover/calendar pattern, not browser-native date inputs as the shipped UI
- no shipped form control may expose browser-default dropdown or calendar chrome

### Select Pattern

```tsx
<Select defaultValue="20">
  <SelectTrigger className="w-24 cursor-pointer">
    <SelectValue />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="10">10</SelectItem>
    <SelectItem value="20">20</SelectItem>
    <SelectItem value="50">50</SelectItem>
    <SelectItem value="100">100</SelectItem>
  </SelectContent>
</Select>
```

Select rules:

- the trigger must be a styled `shadcn/ui` trigger
- the options panel must be a custom `shadcn/ui` floating layer
- the final UI must not fall back to browser-default select styling

### Date Picker Rule

Date selection must not use browser-native picker UI.

Required pattern:

- use `shadcn/ui` trigger styling
- open a custom popover
- render a custom calendar surface
- keep typography, spacing, and border treatment consistent with default `shadcn/ui`

Forbidden shipped UI:

- `<input type="date">`
- `<input type="datetime-local">`
- `<input type="month">`
- `<input type="week">`
- any browser-rendered calendar or date chooser popup

### Image Upload Preview Pattern

This is a required reusable form pattern.

```tsx
<div className="flex flex-wrap gap-3">
  {images.map((image) => (
    <div
      key={image.id}
      className="relative w-20 rounded-lg border border-[#d8dde6] bg-white p-1"
    >
      <button
        type="button"
        className="absolute -right-2 -top-2 flex size-5 cursor-pointer items-center justify-center rounded-full border border-[#d8dde6] bg-white text-[10px] text-slate-700"
        onClick={() => removeImage(image.id)}
      >
        X
      </button>
      <img
        src={image.src}
        alt={image.alt}
        className="h-16 w-full rounded-md object-cover"
      />
      <p className="mt-1 truncate text-[11px] text-slate-500">{image.name}</p>
    </div>
  ))}
</div>
```

Behavior rules:

- selected images appear as small thumbnails
- each thumbnail has a corner remove action
- removing one image removes only that preview item
- previews stay compact and never dominate the form

## Tables

Tables are a primary ERP surface and must stay consistent.

### Required Table Features

- clear headers
- compact rows
- status badges
- gray table header background
- horizontal scrollbar when the table is wider than the viewport or card width
- rows-per-page selector
- pagination

### Table Rules

- table structure should be built from `shadcn/ui` table primitives
- status badges in rows must use the shared badge color mapping
- `Approved` and `Complete` must use success styling
- table header row must use a soft gray background to separate column labels from row data
- the table must remain usable on smaller screens without breaking the layout
- when columns exceed available width, the table must scroll horizontally inside its own container
- pagination and rows-per-page must live in the same control area
- rows-per-page must support:
  - `10`
  - `20`
  - `50`
  - `100`

### Table Header Pattern

```tsx
<Table>
  <TableHeader className="bg-slate-50">
    <TableRow className="border-[#d8dde6] hover:bg-slate-50">
      <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
        Order
      </TableHead>
      <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
        Customer
      </TableHead>
      <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
        Status
      </TableHead>
    </TableRow>
  </TableHeader>
</Table>
```

Header rules:

- header background should be `bg-slate-50` or equivalent soft gray
- header labels should use compact uppercase metadata styling
- header should visually separate labels from body rows immediately

### Table Overflow Pattern

```tsx
<div className="overflow-x-auto rounded-lg border border-[#d8dde6]">
  <Table className="min-w-[720px]">
    <TableHeader className="bg-slate-50">
      <TableRow className="border-[#d8dde6] hover:bg-slate-50">
        <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
          Order ID
        </TableHead>
        <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
          Customer
        </TableHead>
        <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
          Warehouse
        </TableHead>
        <TableHead className="text-xs font-semibold uppercase tracking-[0.08em] text-slate-500">
          Status
        </TableHead>
      </TableRow>
    </TableHeader>
    <TableBody>{rows}</TableBody>
  </Table>
</div>
```

Overflow rules:

- wrap wide tables in `overflow-x-auto`
- give the table a minimum width so columns keep readable spacing
- horizontal scrolling must happen inside the table container, not across the whole page
- the scrollbar should remain visible at the bottom of the table container when overflow exists

### Table Footer Pattern

```tsx
<div className="flex flex-col gap-3 border-t border-[#d8dde6] pt-3 md:flex-row md:items-center md:justify-between">
  <div className="flex items-center gap-2">
    <span className="text-sm text-slate-500">Rows per page</span>
    <Select defaultValue="20">
      <SelectTrigger className="w-24 cursor-pointer">
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="10">10</SelectItem>
        <SelectItem value="20">20</SelectItem>
        <SelectItem value="50">50</SelectItem>
        <SelectItem value="100">100</SelectItem>
      </SelectContent>
    </Select>
  </div>

  <Pagination>
    <PaginationContent>
      <PaginationItem>
        <PaginationPrevious href="#" className="cursor-pointer" />
      </PaginationItem>
      <PaginationItem>
        <PaginationLink href="#" isActive className="cursor-pointer">
          1
        </PaginationLink>
      </PaginationItem>
      <PaginationItem>
        <PaginationNext href="#" className="cursor-pointer" />
      </PaginationItem>
    </PaginationContent>
  </Pagination>
</div>
```

## Global Interaction Rules

- Every clickable element must use `cursor-pointer`
- Shell interactions must feel immediate
- Motion should communicate state, not decoration
- Chevron rotation is a state indicator, not a decorative animation
- Hover states should be subtle and functional
- Menus and dropdowns must use custom `shadcn/ui` surfaces, never browser-native UI
- Browser-native control chrome is not allowed in shipped ERP screens
- Drop shadow and box-shadow are forbidden in all shipped UI components

## Menu Surface Pattern

Use the default `shadcn/ui` pattern for menus.

```tsx
<Menubar>
  <MenubarMenu>
    <MenubarTrigger className="cursor-pointer">View</MenubarTrigger>
    <MenubarContent>
      <MenubarItem className="cursor-pointer">Compact density</MenubarItem>
      <MenubarItem className="cursor-pointer">Audit trail</MenubarItem>
    </MenubarContent>
  </MenubarMenu>
</Menubar>
```

Menu rules:

- menu surfaces should look like `shadcn/ui` defaults
- menu items should use custom hover and focus states
- menu appearance must remain consistent across browsers and operating systems

## Reuse Rules

If you start a new project from this blueprint:

- use `shadcn/ui` as the source for every reusable component by default
- keep the shell structure
- keep the spacing system
- keep the border system
- keep the badge color mapping
- keep full off-canvas sidebar collapse
- keep the compact enterprise interaction style
- do not swap in browser-native UI or a parallel component library where `shadcn/ui` already covers the need
- replace only project-specific data such as:
  - brand name
  - menu tree
  - routes
  - utility actions
  - user identity
  - table data
  - page content

## Implementation Checklist

Before approving a new screen, confirm:

- every reusable UI component is built from `shadcn/ui` primitives or patterns when available
- the shell uses Navbar, Sidebar, and Content in the same structure
- navbar border aligns with sidebar header border
- sidebar collapses fully off-canvas
- sidebar supports direct links and expandable sub-menus
- menu configuration is data-driven, not hardcoded into layout logic
- content uses white cards on the neutral page background
- buttons, badges, and cards follow the shared component rules
- cards do not use shadow as a fake background or primary separation layer
- no component uses drop shadow or box-shadow in any state
- forms include compact operational states
- image upload preview uses thumbnail plus corner remove action
- tables include status consistency, rows-per-page, and pagination
- every clickable element uses `cursor-pointer`
- no component falls back to browser-native UI when a `shadcn/ui` option exists

If any of these fail, the page is no longer a faithful implementation of the blueprint.
