# Project Rules (aitoearn-website specific)

## Prohibited items

- use/( `$`,`USD`,`¥`,`CNY`)-> `appCurrencySymbol` / `appCurrency`(`/src/utils/currency.ts`)
- use `<Input type="number">` `<input type="number">` -> `NumberInput`(`/src/components/ui/number-input.tsx`),,,

## Must-check before development

1. `src/utils/README.md` - utility functions(,OSS,,,)
2. `src/lib/README.md` - Method(toast,confirm,cn,VIP,i18n )
3. `src/hooks/` - custom Hooks(useIsMobile,useMediaUpload,useKeepTimeCountdown,useVideoThumbnail )
4. `src/components/README.md` - shared components(MediaPreview,Modal,LoginModal )
5. `src/store/README.md` - global state management(useUserStore,useAccountStore )
6. `src/app/config/` - business constants and enums(PlatType,PubType,AccountStatus )
7. New** README**

## Project-specific tools

- OSS `getOssUrl`(`/src/utils/oss.ts`)
- Canvas `getOssProxyPath`(`/src/utils/oss.ts`)
- Use for currency display `appCurrencySymbol` + `appCurrency`(`/src/utils/currency.ts`),automatically adapts to domestic/global environments
- SEO `getMetadata`(`/src/utils/general.ts`), title
- store `createPersistStore`(`/src/lib/store.ts`)
- `NumberInput`(`/src/components/ui/number-input.tsx`),use `<Input type="number">`
- :`src/lib/i18n/languageConfig.ts`
- i18n directory:`/src/app/i18n`

## utility functionsspecification

- Method `src/utils/` , `src/utils/README.md`
- addCheck `src/utils/README.md` `src/lib/README.md`,avoid duplicate implementations

## Directory conventions

```
Page-private components -> pages/xxx/components/
shared components -> src/components/
Cross-component state -> xxxStore/
API Method -> src/api/
API Type -> src/api/types/
```

- API Method `src/api/` ,type definitions in `src/api/types/`
- moduleDirectory structure, `src/api/advertiser/xxx.ts` `src/api/types/advertiser/xxx.ts`

## Page-level state management

When multiple components on a page need shared state(,parameters),**use zustand store** ,do not pass large fields layer-by-layer via props.

- `xxxStore.ts`( `promoStore.ts`),use `zustand` + `combine` create local store
- is responsible forInitialization store , store
- Follow when selecting values `useShallow` specification( Store specification)
- Use local store only for page-level shared state; global state remains in `src/store/`

## Config files(`src/app/config/`)

Global business constants and type definitions; review before development to avoid hard-coding or duplicates.

### `platConfig.ts` — Platform configuration (core)

**`PlatType` enum**:all supported social platform identifiers

| Value | Platform | Value | Platform |
| ---------- | ---------- | ----------- | --------- |
| `tiktok` | TikTok | `youtube` | YouTube |
| `douyin` | Douyin | `twitter` | Twitter/X |
| `xhs` | Xiaohongshu | `facebook` | Facebook |
| `wxSph` | WeChat Channels | `instagram` | Instagram |
| `KWAI` | Kwai | `threads` | Threads |
| `bilibili` | Bilibili | `pinterest` | Pinterest |
| `wxGzh` | WeChat Official Account | `linkedin` | LinkedIn |

**`IAccountPlatInfo` interface**:Platformstructure

| Field | Type | Description |
| ----------------------- | -------------- | ---------------------------------------------------------- |
| `themeColor` | `string` | Platform |
| `icon` | `string` | Platform |
| `name` | `string` | Platform(auto i18n via `directTrans`) |
| `url` | `string` | Platform URL |
| `pubTypes` | `Set<PubType>` | Type |
| `commonPubParamsConfig` | `object` | parameters(`titleMax`/`topicMax`/`desMax`/`imagesMax`) |
| `pcNoThis?` | `boolean` | PC hide on PC side |
| `jiancha?` | `boolean` | whether content safety check is required |
| `tips?` | `object` | Platform(`account`/`publish`) |

**`AccountPlatInfoMap`**:`Map<PlatType, IAccountPlatInfo>` — Platform,Platformuse
**`AccountPlatInfoArr`**:Platform,

### `publishConfig.ts` — Type

**`PubType` enum**:

| Value | Description |
| ---------- | ------ |
| `video` | Video |
| `article` | Image-text |
| `article2` | Text only |

### `accountConfig.ts` — Account status

**`AccountStatus` enum**:

| Value | Description |
| ------------- | ------ |
| `1` (USABLE) | Valid |
| `0` (DISABLE) | Invalid |

**`XhsAccountAbnormal` enum**:

| Value | Description |
| -------------- | ------------------------ |
| `1` (Normal) | Account normal |
| `2` (Abnormal) | (Video) |

### `appDownloadConfig.ts` — App download config

- **`MAIN_APP_DOWNLOAD_URL`**:Main app default download URL
- **`getMainAppDownloadUrl(lng?)`**:Async get download URL (prefer API, fallback default)
- **`getMainAppDownloadUrlSync(lng?)`**:Sync version (return cache or default URL)
- **`APP_DOWNLOAD_CONFIGS`**:Platform(`Record<string, AppDownloadConfig>`)
- **`getAppDownloadConfig(platformKey)`**:Platform
- **`fetchLatestDownloadUrl()`**:Fetch latest Android download URL from API (5-minute cache)

### `promotionConfig.ts` — Promotion config

Currently empty file, reserved for promotion-related configs.

---

## Troubleshooting()

### useTransClient Dynamic namespace loading causes flicker

**Cause**:`settings.ts` `common` `route`, namespace use import -> `t('key')` key ( `"setup.title"`)-> ,.

**Scenario 1:Modal/** —

```tsx
// outer layer:( hooks)
export function Modal({ open, onOpenChange }) {
 if (!open) return null
 return <ModalContent onOpenChange={onOpenChange} />
}

// inner layer:use
const ModalContent = memo(({ onOpenChange }) => {
 const { t } = useTransClient('material')
 return (
 <Dialog open onOpenChange={onOpenChange}>
 ...
 </Dialog>
 )
})
```

**Scenario 2: loading ( API )** — outer layer namespace + ready

already has(skeleton / spinner),**outer layer namespace ready**, `skeleton -> key -> ` .

```tsx
const MyComponent = memo(({ id }: Props) => {
 const { initialized, data } = useMyData(id)
 // outer layer namespace,,
 const { ready: i18nReady } = useTransClient('myNamespace')

 // : +
 if (!initialized || !i18nReady) {
 return <LoadingSkeleton />
 }

 // useTransClient('myNamespace') ,ready true
 return data ? <DataView data={data} /> : <EmptyState />
})
```

**Principle**: loading/skeleton ,use namespace,outer layer `useTransClient` `ready` , skeleton -> .

### New Middleware

**Scenario**:NewNo language prefix needed( sitemap,API )

**Cause**:`src/middleware.ts` Path( `/task-sitemap/1` -> `/en/task-sitemap/1`),

**Solution**: `src/middleware.ts` addPath,

### Main page scroll container

 `id="main-content"`( `src/app/layout/MainContent/index.tsx`),Scroll operations like back-to-top should be based on this element, not `window`.
