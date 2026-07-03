# UI Context & Design Tokens

## Aesthetic Overview
CareerSync utilizes a premium dark-mode aesthetic designed to feel technical, highly intelligent, and trustworthy. The interface relies on deep slate/navy backgrounds, subtle glassmorphism for depth, and vibrant electric blue and cyan/teal accents to highlight AI-driven insights and matching scores.

## Color Tokens

| Semantic Token | Hex Value | Role / Usage |
| :--- | :--- | :--- |
| `bg-primary` | `#0B0F19` | The main application background (Deep Navy). |
| `bg-secondary` | `#111827` | Secondary background for sidebars and elevated panels. |
| `surface` | `#1F2937` | Base color for cards and containers (often used with opacity/blur for glassmorphism). |
| `surface-hover` | `#374151` | Interactive state for cards and list items. |
| `border-subtle` | `#374151` | Default borders separating layout sections and subtle dividers. |
| `text-primary` | `#F9FAFB` | Primary text for headings and core readable body text. |
| `text-secondary` | `#9CA3AF` | Muted text for labels, subtitles, and metadata. |
| `brand-primary` | `#3B82F6` | Primary brand color (Electric Blue) for main calls to action and buttons. |
| `brand-foreground` | `#FFFFFF` | Text color that sits on top of the `brand-primary` color. |
| `accent-ai` | `#06B6D4` | AI-specific accent (Cyan/Teal) used for AI insights, skill gap highlights, and AI loading states. |
| `accent-ai-glow` | `#22D3EE` | A lighter cyan variant used for drop-shadows to create a "glowing" effect on AI components. |
| `state-success` | `#10B981` | Emerald Green for high compatibility scores and successful actions. |
| `state-warning` | `#F59E0B` | Amber for warnings, missing skills, or medium match scores. |
| `state-danger` | `#EF4444` | Red for errors or destructive actions. |

## Typography

| Font Role | Font Family | Usage |
| :--- | :--- | :--- |
| **Display / Headings** | `Outfit` or `Plus Jakarta Sans` | Used for all `h1` through `h4` tags. Gives the app a modern, geometric, and technical feel. |
| **Body / UI** | `Inter` | Used for all paragraph text, button labels, and general UI elements. Highly legible at small sizes. |
| **Monospace** | `JetBrains Mono` | Used sparingly for technical outputs, raw data views, or API responses. |

## Border Radius Scale

| Token | Value | Usage |
| :--- | :--- | :--- |
| `rounded-sm` | `0.25rem` (4px) | Small interactive elements, checkboxes, and small tags. |
| `rounded-md` | `0.5rem` (8px) | Standard buttons, text inputs, and dropdown menus. |
| `rounded-lg` | `1rem` (16px) | Main content cards, modals, and glassmorphic panels. |
| `rounded-full` | `9999px` | Avatars, notification dots, and pill-shaped badges. |

## Glassmorphism Notes
To achieve the requested aesthetic, use the `surface` color with a low opacity (e.g., `bg-[#1F2937]/40`) combined with Tailwind's `backdrop-blur-md` utility on top of the `bg-primary` or `bg-secondary` to create depth for floating cards and navigation bars.
