# Recipe: Build a Design System

Create a reusable component library with variables, components, slots, and themes.

## 1. Create the source file

```bash
# Create starter.pen in an empty folder
pen --out starter.pen --prompt "Create a button component with accent color, and a card component with a slot for actions"
```

## 2. Define variables

Open `starter.pen` in pen.dev and add these variables:

| Name | Type | Light | Dark |
|------|------|-------|------|
| `surface` | Color | `#FFFFFF` | `#18181B` |
| `ink` | Color | `#18181B` | `#FAFAFA` |
| `accent` | Color | `#2563EB` | `#2563EB` |
| `space` | Number | 16 | 16 |

## 3. Create components

- **Button**: Frame 144×48, centered label, `accent` fill
- **Card**: Frame 360px wide, vertical flex, `surface` fill, `space` gap/padding
  - Title text: 24px, `ink` fill
  - Detail text: 16px, `ink` fill
  - Actions frame: Make it a **Slot**, suggest Button

## 4. Turn into library

Open Libraries → **Turn this file into a library**

The file becomes `starter.lib.pen`.

## 5. Use in another file

```bash
pen --out consumer.pen --prompt "Import starter.lib.pen, insert a Card, fill the slot with a Button, change the title to 'Ready'"
```

## 6. Update the source

Edit `starter.lib.pen`, change accent color. Save, then reopen `consumer.pen` — changes propagate automatically.