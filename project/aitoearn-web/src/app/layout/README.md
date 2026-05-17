# Layout component documentation

.

## Directory structure

| / | Description |
| ----------------- | ---------------------------- |
| `LayoutSidebar/` | |
| `MobileNav/` | mobile() |
| `Providers.tsx` | Provider |
| `routerData.tsx` | /() |
| `layout.utils.ts` | utility functions |
| `images/` | |

---

## LayoutSidebar -

,use Tailwind CSS + shadcn/ui Implementation.

**Function:**

- Logo
- (use `routerData` )
- Function:,,,/

**:** (md ),mobile

---

## MobileNav - mobile

mobile,use Tailwind CSS + shadcn/ui Implementation.

**Function:**

- (Logo + )
-
- ()

**:** mobile(md ),

---

## routerData -

,.

```tsx
import { routerData } from '@/app/layout/routerData'

interface IRouterDataItem {
 name: string //
 translationKey: string //
 path?: string //
 icon?: React.ReactNode // (Lucide)
 children?: IRouterDataItem[]
}
```

---

## Providers - Provider

```tsx
import { Providers } from '@/app/layout/Providers'

;<Providers lng={lng}>{children}</Providers>
```
