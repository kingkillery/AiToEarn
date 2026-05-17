# PublishDialog

## Overview

PublishDialog ,Platform,.

## ⚠️ :

** PC mobile,ModifyNewFunction！**

| | | Description |
| ------ | ------------------------------------------- | ---------------------------------- |
| PC | `index.tsx` + `DesktopPublishContent` | Function, AI , |
| mobile | `compoents/mobile/MobilePublishContent.tsx` | , AI |

### (index.tsx)

```tsx
const isMobile = useIsMobile()

if (isMobile) {
 return <MobilePublishContent ... /> // mobile
}

// PC
return <DesktopPublishContent ... />
```

## Directory structure

```
PublishDialog/
├── index.tsx # ( + )
├── README.md # this document
├── publishDialog.type.ts # Type
├── PublishDialog.util.ts # utility functions
├── usePublishDialog.ts # store
├── usePublishDialogData.ts # hooks
├── usePublishDialogStorageStore.tsx # store
├── publishDialogTransition.css #
│
├── hooks/ # hooks
│ ├── usePublishState.ts # Modal(loading,modal)
│ ├── useContentModeration.ts #
│ ├── usePlatformAuth.ts # Platform
│ ├── usePublishActions.ts #
│ ├── useAISync.ts # AI
│ ├── useUploadSync.ts # parameters
│ └── usePubParamsVerify.tsx # parametersValidate hook()
│
├── compoents/
│ ├── mobile/
│ │ └── MobilePublishContent.tsx # 【mobile】
│ │
│ ├── DesktopPublishContent/ # 【PC】
│ │ └── index.tsx
│ ├── AccountSelector/ #
│ │ └── index.tsx
│ ├── PublishFooter/ # (,)
│ │ └── index.tsx
│ ├── PublishModals/ # Modal(App,Facebook)
│ │ └── index.tsx
│ │
│ ├── ErrorSummary/ # Wrong()
│ ├── PlatParamsSetting/ # Platformparameters()
│ ├── PublishDatePicker/ # ()
│ ├── PubParmasTextarea/ # ()
│ │
│ ├── PublishDialogAi.tsx # AI ( PC )
│ ├── PublishDialogPreview.tsx # ( PC )
│ ├── TextSelectionToolbar.tsx # ( PC )
│ │
│ ├── Choose/ #
│ ├── DouyinQRCodeModal.tsx # DouyinModal
│ ├── DraftSelectionModal/ # Modal
│ ├── MaterialSelectionModal/ # Modal
│ └── PublishManageUpload/ #
│
└── svgs/ # SVG
```

## Hooks Description

| Hook | |
| ---------------------- | ------------------------------------------------- |
| `usePublishState` | Modal(loading,modal) |
| `useContentModeration` | Function, |
| `usePlatformAuth` | Platform |
| `usePublishActions` | , API |
| `useAISync` | AI (,,) |
| `useUploadSync` | , ossUrl parameters |
| `usePubParamsVerify` | Validateparameters,Wrong |

## Function

| Function | PC | mobile | |
| ------------ | ----- | -------------- | ---------------------- |
| | ✅ | ✅ | `AccountSelector` |
| Wrong | ✅ | ✅ | `ErrorSummary` |
| Platformparameters | ✅ | ✅ | `PlatParamsSetting` |
| | ✅ | ✅ | `PubParmasTextarea` |
| | ✅ | ✅ | `PublishDatePicker` |
| AI | ✅ | ❌ | `PublishDialogAi` |
| | ✅ | ❌ | `PublishDialogPreview` |
| | ✅ | ❌ | `TextSelectionToolbar` |
| | ✅ | ❌ (toast) | - |

##

### :URL parameters()

 `/accounts` parameters,.

**parameters:**

| parameters | Type | Description |
| ------------- | -------- | ---------------------------------------------- |
| `aiGenerated` | `'true'` | ****, AI , |

**parameters:**

| parameters | Type | Description |
| ------------- | ---------- | ------------------------------------- |
| `description` | `string` | Description( URL ) |
| `title` | `string` | ( URL ) |
| `tags` | `string` | JSON ( URL ) |
| `medias` | `string` | JSON ( URL ) |
| `accountId` | `string` | ID |
| `platform` | `PlatType` | PlatformType |
| `taskId` | `string` | Tasks ID |

**use:**

```tsx
// module
const params = new URLSearchParams()
params.set('aiGenerated', 'true')
params.set('description', encodeURIComponent('Description'))
params.set('title', encodeURIComponent(''))
params.set('tags', encodeURIComponent(JSON.stringify(['tag1', 'tag2'])))
params.set('accountId', 'account-123')

router.push(`/accounts?${params.toString()}`)
```

**:** `src/app/[lng]/accounts/accountCore.tsx` 250-330

### :

```tsx
import PublishDialog from '@/components/PublishDialog'

;<PublishDialog
 open={isOpen}
 onClose={() => setIsOpen(false)}
 accounts={accountList}
 defaultAccountIds={['account-1', 'account-2']}
 onPubSuccess={() => console.log('Success')}
/>
```

### : Store

```tsx
import { usePublishDialogStorageStore } from '@/components/PublishDialog/usePublishDialogStorageStore'

//
usePublishDialogStorageStore.getState().setPubData({
 title: '',
 description: 'Description',
 tags: ['tag1', 'tag2'],
 medias: [{ url: '...', type: 'image' }],
})

// ,will automatically
```

## specification

### 1. NewFunctionCheck

NewFunction,****Check:

- [ ] PC Function？-> Modify `DesktopPublishContent`
- [ ] mobileFunction？-> Modify `MobilePublishContent.tsx`
- [ ] ？

### 2. Principle

 `isMobile` prop :

```tsx
interface Props {
 isMobile?: boolean // mobile
}

const MyComponent = ({ isMobile }: Props) => {
 return <div className={isMobile ? 'mobile-style' : 'pc-style'}>...</div>
}
```

### 3.

 store(`usePublishDialog`),.

### 4.

 `hooks/` hook :

```tsx
// ❌ Wrong:
const Component = () => {
 // 100+ ...
}

// ✅ Correct: hook
const Component = () => {
 const { handlePublish, loading } = usePublishActions(...)
}
```

### 5. Wrong

**Wrong**: PC add `ErrorSummary`,mobileadd.

```tsx
// ❌ Wrong: DesktopPublishContent add
<ErrorSummary ... />

// ✅ Correct: MobilePublishContent.tsx add
<ErrorSummary ... />
```

## Data flow

```
┌─────────────────────────────────────────────────────────────┐
│ usePublishDialog (store) │
│ ├── pubList - │
│ ├── pubListChoosed - │
│ ├── step - (0: , 1: ) │
│ ├── expandedPubItem - │
│ ├── commonPubParams - parameters() │
│ └── pubTime - │
└─────────────────────────────────────────────────────────────┘
 │
 ┌───────────────┴───────────────┐
 ▼ ▼
 ┌──────────────────────┐ ┌──────────────────────┐
 │ PC │ │ mobile │
 │ DesktopPublishContent│ │ MobilePublishContent │
 └──────────────────────┘ └──────────────────────┘
```

##

```
┌─────────────────────────────────────────────────────────────────┐
│ │
│ (ShareModal / Agent / Tasks / ) │
└─────────────────────────────────────────────────────────────────┘
 │
 │ router.push('/accounts?aiGenerated=true&...')
 ▼
┌─────────────────────────────────────────────────────────────────┐
│ accountCore.tsx │
│ 1. aiGenerated=true parameters │
│ 2. description/title/tags/medias parameters │
│ 3. usePublishDialogStorageStore.setPubData() │
│ 4. (accountId platform ) │
│ 5. aiGeneratedData │
│ 6. URL parameters │
└─────────────────────────────────────────────────────────────────┘
 │
 │ aiGeneratedData
 ▼
┌─────────────────────────────────────────────────────────────────┐
│ PublishDialog │
│ 1. aiGeneratedData │
│ 2. storage store │
│ 3. │
└─────────────────────────────────────────────────────────────────┘
```

## parametersValidate

```
usePubParamsVerify(pubListChoosed)
 │
 ▼
 ┌─────────────────┐
 │ errParamsMap │ ← Wrong Map<accountId, error>
 │ warningParamsMap│ ← Map<accountId, warning>
 └─────────────────┘
 │
 ▼
 ┌─────────────────┐
 │ ErrorSummary │ ← Wrong/()
 └─────────────────┘
```

## Description

| | |
| ---------------------------------- | --------------------------------------------------- |
| `index.tsx` | , hooks ,Type |
| `usePublishDialog.ts` | ,Method |
| `usePublishDialogStorageStore.tsx` | (IndexedDB) |
| `publishDialog.type.ts` | Type, PubItem,parameters |
| `hooks/usePublishActions.ts` | (API + ) |
| `hooks/useContentModeration.ts` | |
| `hooks/useAISync.ts` | AI (,,) |
| `compoents/DesktopPublishContent/` | PC |
| `compoents/AccountSelector/` | UI |
| `compoents/PublishFooter/` | UI |
| `compoents/PublishModals/` | Modal |

## records

- 2026-01-06:, hooks, 1500 543
- 2026-01-06:New `hooks/` , 6 hooks
- 2026-01-06:New `DesktopPublishContent`,`AccountSelector`,`PublishFooter`,`PublishModals`
- 2026-01-06:add
- 2026-01-06:add `ErrorSummary` mobile
- 2026-01-06:
