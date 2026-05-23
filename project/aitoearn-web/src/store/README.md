# Store - global state management

 [Zustand](https://github.com/pmndrs/zustand) global state managementmodule.

## Directory structure

```
src/store/
├── index.ts #
├── account.ts #
├── user.ts #
├── system.ts #
├── brandInfo.ts #
├── taskSquare.ts # Tasks
├── publishDetailCache.ts #
├── agent/ # AI Agent Tasks
│ ├── index.ts # &
│ ├── agent.types.ts # Type
│ ├── agent.constants.ts #
│ ├── agent.state.ts #
│ ├── agent.methods.ts # Method
│ ├── handlers/ # Action
│ ├── utils/ # utility functions(Refs,,)
│ └── task-instance/ # Tasks(SSE,)
└── plugin/ #
 ├── index.ts #
 ├── store.ts # Store
 ├── hooks.ts # React Hooks
 ├── constants.ts #
 ├── utils.ts # utility functions
 ├── types/ # Type
 └── plats/ # Platform(Douyin,Xiaohongshu)
```

## Store

| | Hook | Function | |
| ----------------------- | ----------------------- | -------------------------------------------- | --------------- |
| `account.ts` | `useAccountStore` | ,,Insufficient balance | |
| `user.ts` | `useUserStore` | ,Credits ,, | |
| `system.ts` | `useSystemStore` | (,) | (IndexedDB) |
| `brandInfo.ts` | `useBrandInfoStore` | (,) | |
| `publishDetailCache.ts` | `usePublishDetailCache` | (5 ) | (IndexedDB) |
| `taskSquare.ts` | `useTaskSquareStore` | Tasks,,Platform, | |
| `notification.ts` | `useNotificationStore` | ,,, | |
| `agent/` | `useAgentStore` | AI Agent Tasks(Tasks,SSE,) | |
| `plugin/` | `usePluginStore` | (,,) | |

## Store Description

### `useAccountStore` —

**:**

- `accountList` / `accountMap` / `accountAccountMap` —
- `accountGroupList` / `accountGroupMap` —
- `accountActive` —
- `activeSpaceId` — ID
- `lowBalanceAlertOpen` — Insufficient balance

**Method:**

- `getAccountList()` — (),Initialization
- `getAccountGroup()` —
- `accountInit()` — Initialization()

---

### `useUserStore` —

**:** `createPersistStore`(localStorage), `creditsInitialized` `creditsLoading` `partialize` ,Value `false` ,Value

**:**

- `token` —
- `userInfo` — (VIP ,Type)
- `creditsBalance` / `creditsLoading` / `creditsInitialized` — Credits
- `lang` —
- `sidebarCollapsed` —
- `defaultPlanId` — ID
- `hasEverLoggedIn` —
- `ipLocation` / `ipLocationUpdatedAt` — IP (24 )
- `countryCode` — ISO alpha-2 ( `"SG"`,`"US"`), IP API ,

**Method:**

- `appInit()` — Initialization( + + + IP )
- `setToken()` —
- `fetchCreditsBalance()` — Credits
- `fetchIpLocation()` — IP + (24 ,Fail)
- `logout()` —

---

### `useSystemStore` —

**:** `createPersistStore`(IndexedDB)

**:**

- `disableLowBalanceAlert` — Insufficient balance
- `calendarViewType` — Type(`'month'` | `'week'`)
- `dismissSeedanceBanner` — Seedance
- `myTasksTab` — Tasks Tab(`'accepted'` | `'published'`, `'accepted'`)

**Method:**

- `setDisableLowBalanceAlert()` —
- `setCalendarViewType()` — Type
- `setDismissSeedanceBanner()` — Seedance
- `setMyTasksTab()` — Tasks Tab

> **:** AI (,,,) `useDraftBoxConfigStore`(`src/app/[lng]/draft-box/draftBoxConfigStore.ts`), groupId .

---

### `useBrandInfoStore` —

**:**

- `brandInfoMap` — `groupId -> BrandLibraryVo | null`
- `loadingMap` / `updatingMap` / `switchingMap` — groupId loading state
- `initializedMap` — groupId Initialization

**Method:**

- `fetchBrandInfo(groupId)` —
- `updateBrandInfo(groupId, data)` —
- `addImage(groupId, url)` / `deleteImage(groupId, imageId)` —
- `linkBrandLib(groupId, placeId)` —
- `switchBrandLib(groupId, placeId)` —

** Hook:**

- `useBrandInfoByGroup(groupId)` —

---

### `usePublishDetailCache` —

**:** `createPersistStore`(IndexedDB)

**:**

- `cache` — `flowId -> PublishRecordItem`
- `timestamps` — `flowId -> Last updated`

**Method:**

- `fetchAndCache(flowId, forceRefresh?)` — (5 )
- `getDetail()` / `setDetail()` —
- `isExpired()` — Check
- `clearCache()` / `clearAllCache()` —

---

### `useNotificationStore` —

**:**

- `notifications` — ()
- `loading` / `loadingMore` — /
- `unreadCount` —
- `pagination` — (page, pageSize, total, hasMore)

**Method:**

- `resetAndFetch()` — +
- `loadMore()` — ()
- `fetchUnreadCount()` —
- `markAsRead(id)` — :,API Fail
- `markAllAsRead()` — :
- `deleteNotification(id)` — :,API Fail

---

### `useAgentStore` — AI Agent Tasks

**:**

- `currentTaskId` — Tasks ID
- `taskMessages` — taskId (,Markdown,,,)
- `currentCost` —
- `pendingTask` — Tasks
- `debugFiles` / `debugMessageIndex` — Debug

**(store Map):**

- `getTaskInstance()` / `getOrCreateTaskInstance()` — /Tasks
- `removeTaskInstance()` / `migrateTaskInstance()` — /Tasks
- `clearAllTaskInstances()` —

**:** `ActionRegistry`,`TaskInstance`,`createAgentRefs`,`createMessageUtils`

---

### `usePluginStore` —

**:**

- `status` — (`UNKNOWN` / `NOT_INSTALLED` / `CHECKING` / `INSTALLED_NO_PERMISSION` / `READY`)
- `isInitializing` — Initialization
- `isPublishing` / `publishingPlatforms` — ()
- `publishProgress` / `platformProgress` —
- `publishTasks` — Tasks
- `platformAccounts` — Platform
- `pluginModalVisible` —

**Method:**

- `init()` — Initialization( -> Check -> )
- `checkPlugin()` / `checkPermission()` — & Check
- `startPolling()` / `stopPolling()` —
- `login()` — Platform
- `publish()` — Platform
- `executePluginPublish()` — (,)
- `syncAccountToDatabase()` —
- `refreshAllPlatformAccounts()` — Platform

**Hooks:** `usePlugin()`,`usePluginLogin()`,`usePluginPublish()`,`usePluginWorkflow()`

---

##

- **normal store:** `zustand` + `combine`
- ** store:** `createPersistStore`(`/src/utils/createPersistStore.ts`)
 - use `localStorage`
 - 4 parameters `'indexedDB'` IndexedDB
- **:** use `useShallow`
