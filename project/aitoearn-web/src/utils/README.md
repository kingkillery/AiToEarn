# Utils utility functions

This directory contains project-wide utility functions.

## File List

| File | Description |
| ----------------------- | ------------------------------------------------ |
| `index.ts` | Utility functions (UUID, time formatting, file parsing, etc.) |
| `format.ts` | Number and date formatting utilities |
| `general.ts` | SEO Metadata |
| `oss.ts` | OSS URL |
| `auth.ts` | Authentication and login redirect helpers |
| `route.ts` | Route accessibility checks for public pages |
| `regulars.ts` | Common regular expressions and validators |
| `agent-asset.ts` | Agent Type |
| `request.ts` | API (get/post/put/delete/patch) |
| `otherRequest.ts` | Unified platform API request wrapper |
| `storage.ts` | LocalStorage wrapper with SSR safety |
| `storageIndexedDb.ts` | IndexedDB storage with SSR fallback support |
| `createPersistStore.ts` | Zustand Store |
| `appLaunch.ts` | App launch helper (iframe + href fallback) |
| `geoData.ts` | Country/state/city data and option builders |
| `download.ts` | Download utilities with progress callbacks |
| `FetchService/` | Fetch |
| `detectPlatform.ts` | Detect social platform from URL (`detectPlatformFromUrl`) |
| `settlement.ts` | CPM/CPE calculation helpers (engagement, estimation) |

---

## index.ts - utility functions

### Import

```typescript
import {
 generateUUID,
 sleep,
 getFilePathName,
 formatTime,
 formatSeconds,
 describeNumber,
 parseTopicString,
 dataURLToBlob,
} from '@/utils'
```

### API

| | Description | Parameters | Value |
| --------------------------- | ----------------------------------------- | --------------------------------------------- | --------------------------- |
| `generateUUID()` | UUID v4 ID | - | `string` |
| `sleep(ms)` | | `ms: number` | `Promise<void>` |
| `getFilePathName(path)` | Pathextract | `path: string` | `{ filename, suffix }` |
| `formatTime(time, format?)` | dayjs | `time`, `format` `'YYYY-MM-DD HH:mm:ss'` | `string` |
| `formatSeconds(seconds)` | `hh:mm:ss` | `seconds: number` | `string` |
| `describeNumber(value)` | Value(k/w) | `value: number` | `string` |
| `parseTopicString(input)` | extract `#` | `input: string` | `{ topics, cleanedString }` |
| `dataURLToBlob(dataURL)` | Base64 DataURL Blob | `dataURL: string` | `Blob` |

### Usage

```typescript
// UUID
const id = generateUUID()

//
await sleep(1000)

//
const { filename, suffix } = getFilePathName('path/to/file.png')
// => { filename: 'file.png', suffix: 'png' }

//
formatTime(new Date(), 'YYYY-MM-DD') // => '2024-01-01'

//
formatSeconds(3661) // => '01:01:01'

// Value
describeNumber(12345) // => '1.23w'
describeNumber(1500) // => '1.5k'

// extract
parseTopicString('Hello #topic1 world #topic2')
// => { topics: ['topic1', 'topic2'], cleanedString: 'Hello world' }
```

---

## format.ts - Formatting Tools

### Import

```typescript
import { formatNumber, formatDate } from '@/utils/format'
```

### API

| | Description | Parameters | Value |
| --------------------------- | ---------------------------------- | ------------------------------------------ | -------- |
| `formatNumber(value)` | Convert large numbers to k/w format (keep 1 decimal) | `value: number` | `string` |
| `formatDate(date, format?)` | Format dates using dayjs | `date`, `format` `'YYYY-MM-DD HH:mm'` | `string` |

### Usage

```typescript
formatNumber(1234) // => '1.2k'
formatNumber(12345) // => '1.2w'

formatDate('2024-01-01') // => '2024-01-01 00:00'
formatDate(Date.now(), 'YYYY/MM/DD') // => '2024/01/01'
```

---

## currency.ts - Currency utilities

### Import

```typescript
import { appCurrency, appCurrencySymbol } from '@/utils/currency'
```

### API

| | Type | Description |
| ------------------------------ | --------------------- | ------------------------------------------------ |
| `appCurrency` | `'CNY' \| 'USD'` | ( CNY / USD) |
| `appCurrencySymbol` | `'¥' \| '$'` | |
| `getCurrencySymbol(currency?)` | `(string?) => string` | , |
| `formatCents(cents)` | `(number) => string` | ,( `500` -> `'5.00'`) |

### Usage

```tsx
;<span>
 {appCurrencySymbol}
 {amount.toFixed(2)} {appCurrency}
</span>
// => ¥100.00 CNY
// => $100.00 USD

//
getCurrencySymbol('CNY') // => '¥'
getCurrencySymbol('USD') // => '$'
getCurrencySymbol() // =>

// (API )
formatCents(500) // => '5.00'
formatCents(1299) // => '12.99'
```

---

## general.ts - SEO

### Import

```typescript
import { getPageTitle, getMetadata } from '@/utils/general'
```

### API

| | Description | Parameters | Value |
| -------------------------------- | -------------------------------------------------------- | ------------------------------------------------- | ------------------- |
| `getPageTitle(name, lng)` | `name —— AiToEarn` | `name: string`, `lng: string` | `Promise<string>` |
| `getMetadata(props, lng, path?)` | SEO Metadata( OG,Twitter Card,hreflang) | `props: Metadata`, `lng: string`, `path?: string` | `Promise<Metadata>` |

### Usage

```typescript
// page.tsx use
export async function generateMetadata({
 params,
}: {
 params: Promise<{ lng: string }>
}): Promise<Metadata> {
 const { lng } = await params
 return getMetadata({ title: 'Pricing', description: 'Pricing' }, lng, '/pricing')
}
```

### Notes

- ⚠️ ** title**,use `getMetadata`

---

## oss.ts - OSS URL

### Import

```typescript
import { getOssUrl } from '@/utils/oss'
```

### API

| | Description | Parameters | Value |
| ------------------ | ---------------------------------------------------- | --------------- | -------- |
| `getOssUrl(path?)` | OSS Path URL( URL ) | `path?: string` | `string` |

### Usage

```typescript
getOssUrl('https://example.com/images/avatar.png')
// => 'https://example.com/images/avatar.png'
```

---

## auth.ts - Authentication

### Import

```typescript
import { navigateToLogin } from '@/utils/auth'
```

### API

| | Description | Parameters | Value |
| ---------------------------- | -------------------------------------------------------- | ------------------- | ------ |
| `navigateToLogin(redirect?)` | ,Success URL() | `redirect?: string` | `void` |

### Usage

```typescript
// ()
navigateToLogin()

//
navigateToLogin('/dashboard')
```

---

## route.ts - Routing

### Import

```typescript
import { isPublicPage } from '@/utils/route'
```

### API

| | Description | Parameters | Value |
| ------------------------ | ------------------------------------ | ------------------ | --------- |
| `isPublicPage(pathname)` | Path() | `pathname: string` | `boolean` |

### Notes

`/auth`,`/websit`,`/blog`,`/chat`,`/promo`,`/welcome`,`/pricing` `/`.

### Usage

```typescript
isPublicPage('/en/pricing') // => true
isPublicPage('/zh-CN/dashboard') // => false
isPublicPage('/') // => true
```

---

## regulars.ts - Regular expressions

### Import

```typescript
import { idNumberReg, phoneReg, emailReg, urlReg } from '@/utils/regulars'
```

### API

| | Description |
| ------------- | ------------------------------------------------------------- |
| `idNumberReg` | Validate(15/18 ) |
| `phoneReg` | Validate( +86 ) |
| `emailReg` | Validate |
| `urlReg` | URL Validate( http/https , `@` Path) |

### Usage

```typescript
emailReg.test('user@example.com') // => true
phoneReg.test('13800138000') // => true
urlReg.test('https://example.com') // => true
```

---

## agent-asset.ts - Agent

 Agent TypeFunction.

### Import

```typescript
import {
 isVideoAssetType,
 isImageAssetType,
 convertAssetToMediaItem,
 convertAssetsToMediaItems,
 filterAssetsByMediaType,
 filterAndConvertAssets,
 getAssetThumbUrl,
 getAssetMediaType,
} from '@/utils/agent-asset'
```

### API

| | Description | Value |
| --------------------------------------------- | ----------------------------- | ------------------ |
| `isVideoAssetType(type)` | VideoType | `boolean` |
| `isImageAssetType(type)` | Type | `boolean` |
| `convertAssetToMediaItem(asset)` | `AssetVo` `MediaItem` | `MediaItem` |
| `convertAssetsToMediaItems(assets)` | | `MediaItem[]` |
| `filterAssetsByMediaType(assets, mediaTypes)` | Type | `AssetVo[]` |
| `filterAndConvertAssets(assets, mediaTypes)` | `MediaItem` | `MediaItem[]` |
| `getAssetThumbUrl(asset)` | URL | `string` |
| `getAssetMediaType(asset)` | Type | `'video' \| 'img'` |

### Usage

```typescript
//
const images = filterAndConvertAssets(assets, 'img')

//
const thumb = getAssetThumbUrl(asset)
```

---

## request.ts - API

 `FetchService` API , Token ,,Wrong 401 .

### Import

```typescript
import request from '@/utils/request'
```

### API

| Method | Description | parameters |
| ---------------------------------------- | ----------- | --------------------------------------------- |
| `request.get<T>(url, data?, silent?)` | GET | `url`, `data`(query parameters), `silent`(Wrong) |
| `request.post<T>(url, data?, silent?)` | POST | `url`, `data`(body), `silent` |
| `request.put<T>(url, data?, silent?)` | PUT | `url`, `data`(body), `silent` |
| `request.delete<T>(url, data?, silent?)` | DELETE | `url`, `data`(body), `silent` |
| `request.patch<T>(url, data?, silent?)` | PATCH | `url`, `data`(body), `silent` |

### Usage

```typescript
// GET
const res = await request.get<UserInfo>('user/info')

// POST
const res = await request.post<void>('user/update', { name: '' })

// (Wrong,)
const res = await request.get<Data>('some/api', null, true)
```

### Notes

- Token ,add
- `silent` , 0 will automatically
- 401 will automatically `logout()`

---

## otherRequest.ts - /Platform API

### Import

```typescript
import { requestPlatApi } from '@/utils/otherRequest'
```

### API

| | Description | Parameters | Value |
| --------------------------- | ---------------------------------- | ----------------------- | ------------ |
| `requestPlatApi<T>(params)` | Platform API ( Token/Wrong) | `params: RequestParams` | `Promise<T>` |

### Usage

```typescript
const data = await requestPlatApi<SomeType>({
 url: 'https://api.platform.com/endpoint',
 method: 'POST',
 data: { key: 'value' },
})
```

---

## storage.ts - LocalStorage

Implementation Zustand `StateStorage` interface LocalStorage ,SSR .

### Import

```typescript
import { appLocalStorage } from '@/utils/storage'
```

### API

| Method | Description |
| ---------------------- | ----------------------- |
| `getItem(name)` | Value(SSR null) |
| `setItem(name, value)` | Value(SSR ) |
| `removeItem(name)` | Value(SSR ) |

### Notes

- use, `createPersistStore`

---

## storageIndexedDb.ts - IndexedDB

Implementation Zustand `StateStorage` interface IndexedDB ,SSR , LocalStorage.

### Import

```typescript
import { indexedDBStorage } from '@/utils/storageIndexedDb'
```

### API

| Method | Description |
| ---------------------- | ---------------------------------------------- |
| `getItem(name)` | Value(SSR null, localStorage) |
| `setItem(name, value)` | Value( hydrate ) |
| `removeItem(name)` | Value |
| `clear()` | |

### Notes

- use, `createPersistStore`
- use `idb-keyval` IndexedDB

---

## createPersistStore.ts - Zustand Store

 Zustand Store, LocalStorage IndexedDB .

### Import

```typescript
import { createPersistStore } from '@/utils/createPersistStore'
```

### API

```typescript
function createPersistStore<T, M>(
 state: T, //
 methods: (set, get) => M, // Store Method
 persistOptions: PersistOptions, // (name )
 type?: 'localStorage' | 'indexedDB' // Type, localStorage
)
```

### Method

Store Method:

| Method | Description |
| ----------------------- | ------------------------------------ |
| `update(updater)` | (Modify) |
| `markUpdate()` | |
| `setHasHydrated(state)` | hydration |

### Usage

```typescript
const useMyStore = createPersistStore(
 { count: 0, name: '' },
 (set, get) => ({
 increment() {
 set({ count: get().count + 1 })
 },
 }),
 { name: 'my-store' },
 'localStorage'
)

// use update Modify
useMyStore.getState().update((state) => {
 state.name = ''
})
```

---

## FetchService/ - Fetch

 `fetch` ,/,parameters.

### Import

```typescript
import FetchService from '@/utils/FetchService/FetchService'
import type { RequestParams, IFetchServiceConfig } from '@/utils/FetchService/types'
```

### Type

```typescript
interface RequestParams extends RequestInit {
 url: string
 method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH'
 data?: Dictionary // body ( JSON , FormData)
 params?: Dictionary // query parameters( URL)
}

interface IFetchServiceConfig<T = Response> {
 baseURL: string
 requestInterceptor?: (requestParams: RequestParams) => RequestParams | void | null
 responseInterceptor?: (response: Response) => T
}
```

### Usage

```typescript
const service = new FetchService({
 baseURL: 'https://api.example.com/',
 requestInterceptor(params) {
 params.headers = { ...params.headers, Authorization: 'Bearer xxx' }
 return params
 },
 responseInterceptor(response) {
 return response
 },
})

const res = await service.request({
 url: 'users',
 method: 'GET',
 params: { page: 1 },
})
```

### Notes

- use, `request.ts` `otherRequest.ts`
- `FormData` ( JSON )
- `data` Value `undefined` Field

---

## media.ts - utility functions

### Import

```typescript
import { getVideoDuration, getVideoInfo, formatVideoDuration } from '@/utils/media'
```

### API

| | Description | Parameters | Value |
| ------------------------------ | -------------------------------------------------------- | ----------------- | ------------------------------------------------- |
| `getVideoDuration(file)` | Video(), video metadata | `file: File` | `Promise<number>` |
| `getVideoInfo(file)` | Videoextract(data URL) | `file: File` | `Promise<{ coverUrl: string; duration: number }>` |
| `formatVideoDuration(seconds)` | Video `M:SS` | `seconds: number` | `string` |

### Usage

```typescript
const duration = await getVideoDuration(videoFile)
// => 15.3

const info = await getVideoInfo(videoFile)
// => { coverUrl: 'data:image/jpeg;base64,...', duration: 15.3 }

formatVideoDuration(65) // => '1:05'
formatVideoDuration(7) // => '0:07'
```

---

## geoData.ts - Geographic data utilities

### Import

```typescript
import {
 getCountryOptions,
 getStateOptions,
 getCityOptions,
 getCountryDisplayName,
} from '@/utils/geoData'
```

### API

| | Description | Parameters | Value |
| ----------------------------------------- | --------------------------------------- | ----------------------------------------------- | ----------------- |
| `getCountryOptions(lng)` | , | `lng: string` | `CountryOption[]` |
| `getStateOptions(countryCode)` | /, | `countryCode: string` | `StateOption[]` |
| `getCityOptions(countryCode, stateCode)` | / | `countryCode: string, stateCode: string` | `CityOption[]` |
| `getCountryDisplayName(countryCode, lng)` | ISO code | `countryCode: string \| undefined, lng: string` | `string` |

### Usage

```typescript
//
const countries = getCountryOptions('zh-CN') // => [{ value: 'CN', label: '' }, ...]

// /( -> UI )
const states = getStateOptions('US') // => [{ value: 'CA', label: 'California' }, ...]

//
const cities = getCityOptions('US', 'CA') // => [{ value: 'Los Angeles', label: 'Los Angeles' }, ...]

// ()
getCountryDisplayName('US', 'zh-CN') // => ''
getCountryDisplayName('US', 'en') // => 'United States'
```

### Notes

- :`country-state-city` npm + `src/data/countries_alpha2.json`()
- /,UI

---

## appLaunch.ts - App launch

### Import

```typescript
import { openApp } from '@/utils/appLaunch'
```

### API

| | Description | Parameters | Value |
| --------------------------- | ------------------------------------------------- | ---------------------------------------- | ------ |
| `openApp(apiUrl, onFailed)` | App(API 302 scheme URL) | `apiUrl: string`, `onFailed: () => void` | `void` |

### Notes

1. **iframe **: iframe, 302 scheme URL
2. **location.href **: 200ms, iframe
3. ****:2.5s `visibilitychange` Success,Fail `onFailed`

### Usage

```typescript
openApp('https://api.example.com/redirect', () => {
 console.log('Fail')
})
```

---

## download.ts - Download utilities

### Import

```typescript
import { fetchWithProgress } from '@/utils/download'
```

### API

| | Description | Parameters | Value |
| ------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------- | --------------- |
| `fetchWithProgress(url, onProgress?)` | fetch , ReadableStream | `url: string`, `onProgress?: (progress: number) => void` | `Promise<Blob>` |

### Usage

```typescript
//
const blob = await fetchWithProgress('https://example.com/file.zip', (progress) => {
 console.log(`${progress}%`) // 0-100
})

//
const blob = await fetchWithProgress('https://example.com/file.zip')
```

### Notes

- `Content-Length` ( blob, 100%)
- Value 0-100

---

## settlement.ts - CPM/CPE

### Import

```typescript
import {
 ENGAGEMENT_WEIGHTS,
 calculateEngagementScore,
 calculateEstimatedAmount,
} from '@/utils/settlement'
```

### API

| / | Description | parameters | Value |
| ------------------------------------------------------ | ---------------------------------------------- | ---------------------------------------------------------------------------- | -------- |
| `ENGAGEMENT_WEIGHTS` | CPE (LIKE=1, COLLECT=3, COMMENT=5) | - | `object` |
| `calculateEngagementScore(likes, favorites, comments)` | | `likes, favorites, comments: number` | `number` |
| `calculateEstimatedAmount(count, pricePerThousand)` | () | `count: number` (/), `pricePerThousand: number` (,) | `number` |

### Usage

```typescript
// = 234×1 + 89×3 + 56×5 = 781
const score = calculateEngagementScore(234, 89, 56)

// CPM = (12345 / 1000) × 100 = 1235
const cpmAmount = calculateEstimatedAmount(12345, 100)

// CPE = (781 / 1000) × 500 = 391
const cpeAmount = calculateEstimatedAmount(score, 500)
```

---

## Newutility functionsspecification

addutility functions,:

1. Checkthis document `src/lib/README.md`,already existsFunction
2. ()
3. add JSDoc
4. this document
