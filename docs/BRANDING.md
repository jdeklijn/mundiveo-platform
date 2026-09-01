# MundiVeo — Branding

**Source of truth for visual identity.** Logo and color palette are fixed assets — reference them, don't reinterpret or regenerate them.

## Logo

`assets/mundiveo-logo.jpg` — navy wordmark with globe/play icon.

## Color Palette

| Name | Hex | Usage |
|---|---|---|
| Primary Navy | `#0F2C4C` | Primary brand color, logo, headers, primary buttons |
| Secondary Slate Blue | `#3A5A7A` | Secondary accents, links, hover states |
| Accent Teal | `#00C2A8` | Call-to-action highlights, active states, notifications |
| Neutrals White | `#FFFFFF` | Base background (light mode) |
| Off-white | `#F7F9FC` | Card/section backgrounds (light mode) |
| Light Gray | `#E8EEF4` | Borders, dividers, subtle backgrounds (light mode) |
| Dark Text | `#1A1F2E` | Primary text (light mode) |
| Medium Text | `#5A6A7A` | Secondary/muted text (light mode) |
| Dark Mode Background | `#0B1520` | Base background (dark mode) |
| Dark Mode Surface | `#152535` | Card/section backgrounds (dark mode) |

Full palette reference image: `assets/mundiveo-color-palette.jpg`

## Tailwind config reference

Use these as the basis for `tailwind.config.js` theme colors — don't invent additional brand colors without updating this file first:

```js
colors: {
  navy: '#0F2C4C',
  slate: '#3A5A7A',
  teal: '#00C2A8',
  offwhite: '#F7F9FC',
  lightgray: '#E8EEF4',
  darktext: '#1A1F2E',
  mediumtext: '#5A6A7A',
  darkbg: '#0B1520',
  darksurface: '#152535',
}
```
