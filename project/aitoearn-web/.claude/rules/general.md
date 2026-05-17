# General rules (for all Next.js projects)

Answer questions in Chinese.

Do not write in commits "Co-Authored-By" "<noreply@anthropic.com>"

## Skills use

**Must automatically invoke the following Skills:**

| Skill | Scenario |
| ----------------------------- | -------------------------------------- |
| `vercel-react-best-practices` | When writing, reviewing, or refactoring React/Next.js code |
| `frontend-design` | ,Modify, UI () |

## Prohibited items

- use `npm run build` TypeCheck -> `npx tsc --noEmit`
- use( `text-gray-900`)-> shadcn/ui
- use(`#000`,`black`,`text-black`)-> (`text-muted-foreground`,`text-foreground` )
- `as any` TypeScript TypeWrong -> Type( interface/type,addType)
- use `<Input type="number">` `<input type="number">` -> `NumberInput` (`src/components/ui/number-input.tsx`)

## Code reuse (mandatory)

**Principle:Function,Implementation.**

### Mandatory pre-coding search process

Before writing new logic each time,**must search in order**,Implementation:

1. **utility functions**:`src/utils/README.md` -> `src/lib/README.md` ->
2. **custom Hooks**:`src/hooks/` all hooks in the directory
3. **shared components**:`src/components/README.md` ->
4. ****:`src/app/config/` Config files
5. **Store Method**:`src/store/README.md` -> store already has/Method
6. **Page-level reuse**: store,hooks,utils

**If similar implementation exists, must reuse or extend; do not start from scratch.**

### Forbidden duplicate patterns

```tsx
// ❌ :handwritten,already has formatNumber / formatDate / formatTime / formatSeconds
const formatted = value > 10000 ? (value / 10000).toFixed(1) + 'w' : value

// ❌ :handwritten,already has appCurrencySymbol / formatCents
const price = '$' + (amount / 100).toFixed(2)

// ❌ :handwritten OSS URL ,already has getOssUrl / getOssProxyPath
const imgUrl = 'https://oss.xxx.com/' + path

// ❌ :handwrittenmobile,already has useIsMobile hook
const [isMobile, setIsMobile] = useState(window.innerWidth < 768)

// ❌ :handwrittenVideo,already has getVideoDuration / getVideoInfo
const video = document.createElement('video')

// ❌ :handwritten,already has useKeepTimeCountdown hook
const [countdown, setCountdown] = useState(60)

// ❌ :handwritten VIP ,already has getVipStatusInfo / getVipTier
const isVip = status === 'active_monthly' || status === 'active_yearly'

// ❌ :handwritten,already has isChineseLanguage
const isCN = lng === 'zh-CN' || lng === 'zh-TW'

// ❌ :handwritten,already has formatRelativeTime
const timeAgo = Date.now() - time < 60000 ? 'just now' : ...

// ❌ :handwrittenModal,already has confirm()
if (window.confirm('Confirm deletion?')) { ... }

// ❌ :use type="number",(,spinner )
<Input type="number" onChange={e => field.onChange(Number(e.target.value))} />

// ✅ Correct:use NumberInput,,,
<NumberInput value={field.value} onValueChange={v => field.onChange(v)} decimalScale={2} />
```

### Similar UI patterns must be extracted and reused

- **List page**:,empty state,loading skeleton -> Check
- **Form page**:Validate,,loading -> Check `react-hook-form` + `zod` already has
- **Modal**:use `src/components/ui/modal.tsx` `confirm()`,Modal
- **Media preview**:use `MediaPreview` ,/Video

### New vs reuse decision

| Scenario | Approach |
| -------------------- | ----------------------------------------------- |
| Function | **Use directly** |
| Function 80% | **Extend existing**(parameters/) |
| Function 50% | **Extract common parts**/ |
| Function,Implementation | **Allow new creation**, README |

### When duplicate code is found

- already exists,**proactively remind user**
- New(utils / hooks / components),**** README

## Code quality

- already hasType
- lint/prettier/typescript specification
- :,FunctionDescription
- API Add loading for operations and skeleton for UI
- Button loading state:`Loader2` spinning icon(`lucide-react`)+ `disabled`,do not change button text
- `cursor: pointer`,compatible with mobile

## UI Style

- :Tailwind CSS + shadcn/ui
- Color tone:use Tailwind Color tone(`slate`/`gray`/`neutral`),keep a soft and comfortable visual feel
- use(`#000` / `black`),use( `foreground`/`muted-foreground`)
- Style:,,,
- use shadcn/ui,

## Component folder conventions

use,Function, `index.tsx` .

```
// ❌ Wrong: components/
components/
 DraftDetailDialog.tsx
 DraftDetailDialog.module.scss

// ✅ Correct:
components/
 DraftDetailDialog/
 index.tsx
 DraftDetailDialog.module.scss
```

- Main component file named `index.tsx`,style file named `[].module.scss`
- ,hooks,utility functions
- use(PascalCase),

## API file conventions

- API MethodType****,
- interfaceValue `code === 0` Success, code Fail, `message`

```ts
// interfaceResponse format
{ "data": {}, "code": 0, "message": "success", "timestamp": 1772099056662 }

// ✅ Correct: code === 0
const res = await createXxx(params)
if (res.code === 0) {
 toast.success(t('createSuccess'))
} else {
 toast.error(res.message)
}

// ❌ Wrong: code,Success
const res = await createXxx(params)
toast.success(t('createSuccess'))
```

## Tailwind CSS v4

```tsx
// ✅ Correct
<div className="bg-(--primary-color)" />
// ❌ Wrong
<div className="bg-[var(--primary-color)]" />
```

## Store specification

- normal store:`zustand` + `combine`
- ****use `useShallow`( `zustand/react/shallow`) store propertyValue,
- single selector can omit useShallow; **2 or more** must be combined
- child components get data directly from store

```tsx
// ❌ Wrong:
const foo = useXxxStore((state) => state.foo)
const bar = useXxxStore((state) => state.bar)

// ✅ Correct:useShallow
const { foo, bar } = useXxxStore(
 useShallow((state) => ({
 foo: state.foo,
 bar: state.bar,
 }))
)
```

## Internationalization

- Internationalization
- `t()` Type
- New key

## specification

```tsx
// ✅ No language prefix needed
<Link href="/pricing">Pricing</Link>
// ❌ Wrong
<Link href={`/${lng}/pricing`}>Pricing</Link>
```

---

# Next.js 14 specification

## App Router

- use,
- use `'use client'`
- use `error.tsx` Wrong,`loading.tsx` manage loading state
- use Next.js Image

## Data fetching

```tsx
async function getData() {
 const res = await fetch('https://api.example.com/data', {
 next: { revalidate: 3600 },
 })
 if (!res.ok) throw new Error('Failed to fetch data')
 return res.json()
}
```

##

```tsx
//
export const metadata: Metadata = { title: 'Page Title' }

//
export async function generateMetadata({
 params,
}: {
 params: Promise<{ id: string }>
}): Promise<Metadata> {
 const { id } = await params
 return { title: data.title }
}
```

---

# TypeScript specification

##

```tsx
interface ComponentNameProps {
 prop1: string
}

const ComponentName = ({ prop1 }: ComponentNameProps) => {
 // component logic
}

export const ComponentName = () => {} // named export
export default Page // default page export
```

- TypeScript Type,Type
- use `JSX.Element`, `React.ReactNode`

---

# SCSS specification

## :global() syntax

```scss
// ❌ Wrong:cannot use &
:global(.className) {
 &-loaded {
 opacity: 1;
 }
}

// ✅ Correct:declare separately
:global(.className) {
 opacity: 0;
}
:global(.className-loaded) {
 opacity: 1;
}
```

## BEM naming

- Block: `.block`
- Element: `.block_element`()
- Modifier: `.block_element-modifier`()

---

# Troubleshootingrecords

## 1. Radix UI Popover Invalid

**Cause**:`react-remove-scroll` blocks internal scrolling

**Solution**: `PopoverContent` add `allowInnerScroll` property

```tsx
<PopoverContent allowInnerScroll>
 <div className="overflow-y-auto">...</div>
</PopoverContent>
```

## 2. Canvas cross-origin issue

**Cause**:cross-origin images taint Canvas

**Solution**:use

## 3. Radix UI Tooltip controlled/uncontrolled switch warning

**Cause**:conditionally `open` `false` `undefined`

**Wrong**:

```tsx
<Tooltip open={someCondition ? false : undefined}>
```

**Solution**:use, state Tooltip

```tsx
const [tooltipOpen, setTooltipOpen] = useState(false)

<Tooltip open={!someCondition && tooltipOpen} onOpenChange={setTooltipOpen}>
```

## 4. `confirm()` Modal loading , `onOk`

**Scenario**:Modal API,Modal loading

**WrongApproach**:`confirm` returns boolean -> Modal -> loading(disjointed UX)

```tsx
// ❌ Wrong:API confirm ,Modal loading
const confirmed = await confirm({ title: '？' })
if (!confirmed) return
setLoading(true)
await apiCall()
setLoading(false)
```

**CorrectApproach**:place API call into `onOk` ,`confirm` will automaticallyshow loading on confirm button,API Modal

```tsx
// ✅ Correct:API onOk ,Modal loading
await confirm({
 title: '？',
 onOk: async () => {
 await apiCall()
 // Success...
 },
})
```

This way no need to maintain extra `xxxLoading` state,also no need to `loading` prop.
