# Lib

utility functions.

##

| | Description |
| ------------------------ | ------------------------------------------- |
| `toast.ts` | Toast , antd message |
| `confirm.tsx` | Modal, antd Modal.confirm |
| `utils.ts` | utility functions |
| `vip.ts` | VIP utility functions |
| `i18n/languageConfig.ts` | , |

---

## toast.ts -

 [sonner](https://sonner.emilkowal.ski/) Toast ,** antd `message` **.

###

```typescript
import { toast } from '@/lib/toast'
//
import toast from '@/lib/toast'
```

### API

| Method | Description | parameters |
| ---------------------------------- | --------------------------- | ------------------- |
| `toast.success(content, options?)` | Success() | `content`: |
| `toast.error(content, options?)` | Wrong() | `content`: |
| `toast.warning(content, options?)` | () | `content`: |
| `toast.info(content, options?)` | () | `content`: |
| `toast.loading(content, options?)` | ( loading ) | `content`: |
| `toast.open(options)` | Type( antd) | |
| `toast.destroy(key?)` | / | `key`: ID |
| `toast.dismiss(key?)` | /() | `key`: ID |
| `toast.dismissAll()` | | - |

### Options parameters

```typescript
type ToastOptions = {
 key?: string // (/)
 id?: string // key
 duration?: number // (),loading Infinity
 content?: React.ReactNode // ()
}
```

### use

```typescript
//
toast.success('Success')
toast.error('Fail')
toast.warning('')
toast.info('')

// options
toast.success('Success', { duration: 5 })

// loading
const toastId = toast.loading('...')
//
toast.dismiss(toastId)

// antd message.open
toast.open({
 type: 'success',
 content: 'Success',
 key: 'unique-key',
 duration: 3,
})
```

### Notes

- ⚠️ **use antd `message` **,use
- `duration` parameters****()
- `loading` Typewill automatically, `dismiss`

---

## confirm.tsx - Modal

 [shadcn/ui AlertDialog](https://ui.shadcn.com/docs/components/alert-dialog) Modal,** antd `Modal.confirm` Method**.

###

```typescript
import { confirm } from '@/lib/confirm'
```

### API

| parameters | Description | Type | Value |
| ------------------- | -------------------- | --------------------------- | ------------ |
| `title` | | `React.ReactNode` | `"Confirm"` |
| `content` | Description | `React.ReactNode` | - |
| `okText` | | `React.ReactNode` | `"Confirm"` |
| `cancelText` | | `React.ReactNode` | `"Cancel"` |
| `onOk` | () | `() => Promise<any> \| any` | - |
| `onCancel` | () | `() => Promise<any> \| any` | - |
| `okButtonProps` | property | `ButtonProps` | - |
| `cancelButtonProps` | property | `ButtonProps` | - |
| `icon` | | `React.ReactNode` | |

### Value

 `Promise<boolean>`:

- `true`:
- `false`:

### use

```typescript
//
const result = await confirm({
 title: '',
 content: '？.',
})

if (result) {
 //
 await deleteItem()
}

//
await confirm({
 title: '',
 content: '？',
 okText: '',
 cancelText: '',
 onOk: async () => {
 await submitData()
 },
})

//
await confirm({
 title: '',
 content: '',
 okButtonProps: {
 className: 'bg-red-500 hover:bg-red-600',
 },
})
```

### Notes

- ⚠️ **use antd `Modal.confirm`**,use
- ,will automatically DOM
- `onOk` loading
- `onOk` ,Promise reject

---

## utils.ts - utility functions

### cn -

 Tailwind CSS ,. `clsx` + `tailwind-merge`.

####

```typescript
import { cn } from '@/lib/utils'
```

#### use

```typescript
//
cn('px-4 py-2', 'bg-blue-500')
// => 'px-4 py-2 bg-blue-500'

//
cn('base-class', isActive && 'active-class')
// => 'base-class active-class' 'base-class'

// syntax
cn('base', { 'text-red-500': hasError, 'text-green-500': !hasError })

// Tailwind
cn('px-4', 'px-6')
// => 'px-6' ()

// use
<div className={cn('default-styles', className, {
 'opacity-50': disabled
})}>
```

#### use cn？

### formatRelativeTime -

,"just now","5","3".

####

```typescript
import { formatRelativeTime } from '@/lib/utils'
```

#### use

```typescript
// Date
formatRelativeTime(new Date())
// => 'just now'

//
formatRelativeTime(Date.now() - 60000)
// => '1'

// 7 Date
formatRelativeTime(new Date('2024-01-01'))
// => '2024-01-01'
```

#### Value

| | Value |
| --------- | ------------ |
| < 1 | "just now" |
| < 1 | "X" |
| < 24 | "X" |
| < 7 | "X" |
| >= 7 | "YYYY-MM-DD" |

1. **Solution**:`tailwind-merge` Tailwind
2. ****:`clsx` ,,syntax
3. **Type**: TypeScript

---

## NewMethodspecification

addMethod,:

1. Checkthis documentalready existsFunction
2. ()
3. add JSDoc
4. this document

---

## vip.ts - VIP

 VIP ,.

###

```typescript
import { getVipStatusInfo, getVipTier, canUpgrade } from '@/lib/vip'
import type { VipStatusInfo, VipTier } from '@/lib/vip'
```

### Type

```typescript
//
type VipTier = 'free' | 'go' | 'plus' | 'ultra'

// VIP
interface VipStatusInfo {
 isVip: boolean // VIP
 isMonthly: boolean //
 isYearly: boolean //
 isAutoRenew: boolean //
 isOnce: boolean //
}
```

### API

#### getVipStatusInfo(status: string): VipStatusInfo

 VIP .

#####

| Value | isVip | isMonthly | isYearly | isAutoRenew | isOnce |
| -------------------- | ----- | --------- | -------- | ----------- | ------ |
| `none` | false | false | false | false | false |
| `trialing` | true | false | false | false | false |
| `monthly_once` | true | true | false | false | true |
| `yearly_once` | true | false | true | false | true |
| `active_go` | true | false | false | true | false |
| `active_monthly` | true | true | false | true | false |
| `active_yearly` | true | false | true | true | false |
| `active_ultra` | true | false | false | true | false |
| `active_nonrenewing` | true | false | false | false | false |
| `expired` | false | false | false | false | false |

#### getVipTier(status: string): VipTier

 VIP .

#####

| Value | |
| ---------------------------------------------------------------- | ------------------------------- |
| `active_go` | `'go'` |
| `active_monthly`, `monthly_once`, `active_yearly`, `yearly_once` | `'plus'` |
| `active_ultra` | `'ultra'` |
| `trialing` | `'plus'` ( Plus ) |
| | `'free'` |

#### canUpgrade(tier: VipTier): boolean

.

| | |
| ------- | -------------------------- |
| `free` | false |
| `go` | true ( Plus/Ultra) |
| `plus` | true ( Ultra) |
| `ultra` | false () |

### use

```typescript
import { getVipStatusInfo, getVipTier, canUpgrade } from '@/lib/vip'

//
const statusInfo = getVipStatusInfo(userInfo.vipInfo.status)

if (statusInfo.isVip) {
 // VIP
}

if (statusInfo.isMonthly && statusInfo.isAutoRenew) {
 //
}

//
const tier = getVipTier(userInfo.vipInfo.status)
// => 'go' | 'plus' | 'ultra' | 'free'

//
if (canUpgrade(tier)) {
 //
}
```

---

## i18n/languageConfig.ts -

,.

###

```typescript
import {
 getDayjsLocale,
 getDateLocale,
 getHreflang,
 getAllLanguageOptions,
 isChineseLanguage,
} from '@/lib/i18n/languageConfig'
```

### API

| | Description | Value |
| -------------------------- | ------------------------------------- | ------------------------------------ |
| `getDayjsLocale(lng)` | dayjs/FullCalendar use locale | `string`( 'zh-cn', 'en-gb') |
| `getDateLocale(lng)` | toLocaleDateString use locale | `string`( 'zh-CN', 'en-US') |
| `getHreflang(lng)` | SEO hreflang propertyValue | `string`( 'zh', 'en') |
| `getAllLanguageOptions()` | () | `{ value: string, label: string }[]` |
| `isChineseLanguage(lng)` | | `boolean` |
| `isSupportedLanguage(lng)` | | `boolean` |

### use

```typescript
// dayjs locale
const dayjsLocale = getDayjsLocale(lng)
dayjs.locale(dayjsLocale)

// Date
const dateStr = new Date().toLocaleDateString(getDateLocale(lng), {
 year: 'numeric',
 month: 'long',
 day: 'numeric',
})

//
const options = getAllLanguageOptions()
// => [{ value: 'en', label: 'English' }, { value: 'zh-CN', label: '' }]

// SEO hreflang
const alternates = languages.map((lang) => ({
 href: `${baseUrl}/${lang}`,
 hreflang: getHreflang(lang),
}))

//
if (isChineseLanguage(lng)) {
 //
}
```

### add

 `src/lib/i18n/languageConfig.ts` `LANGUAGE_METADATA` add:

```typescript
export const LANGUAGE_METADATA: Record<string, LanguageMetadata> = {
 // ...
 ja: {
 code: 'ja',
 label: '',
 dayjsLocale: 'ja',
 hreflang: 'ja',
 dateLocale: 'ja-JP',
 },
}
```

### Notes

- ⚠️ ****,use
- add, `src/app/i18n/settings.ts` `languages` add
