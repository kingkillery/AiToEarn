# Platformmodule (plats)

PlatformmoduleinterfaceImplementationPlatform,,,Function.

## Directory structure

```
plats/
├── types.ts # Type
├── manager.ts # Platform
├── index.ts # module
├── README.md # this document
├── demo/ # ()
├── douyin/ # DouyinPlatform
│ ├── index.ts #
│ ├── types.ts # DouyinType
│ └── homeFeed.ts # Functionmodule
└── xhs/ # XiaohongshuPlatform
 ├── index.ts #
 ├── types.ts # XiaohongshuType
 └── homeFeed.ts # Functionmodule
```

## Principle

1. **interface**:PlatformImplementation `IPlatformInteraction` interface,Implementation
2. **module**:PlatformFunctionmodule,
3. ****:Platform API
4. ****:,
5. ****:addPlatformFunctionImplementationmodule

## interface

```typescript
interface IPlatformInteraction {
 readonly platformType: PlatType

 //
 likeWork(workId: string, isLike: boolean): Promise<LikeResult>
 commentWork(params: CommentParams): Promise<CommentResult>
 favoriteWork(workId: string, isFavorite: boolean): Promise<FavoriteResult>

 // Function
 sendDirectMessage?(params: DirectMessageParams): Promise<DirectMessageResult>

 //
 getHomeFeedList(params: HomeFeedListParams): Promise<HomeFeedListResult>
}
```

## PlatformFunction

| Platform | | | | | |
| ------ | ------ | ------ | ------ | ------ | -------- |
| Xiaohongshu | API | API | API | - | API |
| Douyin | | | | | API |

###

- **API **:,,
- ****:,,

## use

### :()

```typescript
import { platformManager } from '@/store/plugin/plats'
import { PlatType } from '@/app/config/platConfig'

//
await platformManager.likeWork(PlatType.Douyin, workId, true)

//
await platformManager.commentWork(PlatType.Xhs, { workId, content: '' })

//
await platformManager.favoriteWork(PlatType.Douyin, workId, true)

//
const result = await platformManager.getHomeFeedList(PlatType.Xhs, { page: 1, size: 20 })
if (result.success) {
 console.log(':', result.items)
}
```

### :usePlatform

```typescript
import { douyinInteraction, xhsInteraction } from '@/store/plugin/plats'

await douyinInteraction.likeWork(workId, true)
await xhsInteraction.commentWork({ workId, content: '' })

//
const result = await xhsInteraction.getHomeFeedList({ page: 1, size: 20 })
```

##

```typescript
interface HomeFeedItem {
 workId: string // ID
 thumbnail: string // URL
 title: string //
 authorAvatar: string //
 authorName: string //
 authorId: string // ID
 likeCount: string // ("")
 isVideo: boolean // Video
 videoDuration?: number // Video()
 origin: any // Platform
}
```

## Platform

### 1:PlatformDirectory structure

```
plats/kuaishou/
├── index.ts #
├── types.ts # KwaiType
└── homeFeed.ts # Functionmodule
```

### 2:PlatformType

```typescript
// plats/kuaishou/types.ts

/** KwaiType */
export interface KuaishouHomeFeedResponse {
 success: boolean
 data?: {
 items: KuaishouHomeFeedItem[]
 cursor?: string
 }
}

/** Kwai */
export interface KuaishouHomeFeedItem {
 id: string
 // ... Field
}
```

### 3:ImplementationFunctionmodule

```typescript
// plats/kuaishou/homeFeed.ts
import type { HomeFeedListParams, HomeFeedListResult } from '../types'

//
class HomeFeedCursorManager {
 /* ... */
}
export const homeFeedCursor = new HomeFeedCursorManager()

//
export function transformToHomeFeedItem(item: KuaishouHomeFeedItem): HomeFeedItem {
 return {
 /* */
 }
}

//
export async function getHomeFeedList(params: HomeFeedListParams): Promise<HomeFeedListResult> {
 // Implementation
}
```

### 4:

```typescript
// plats/kuaishou/index.ts
import { PlatType } from '@/app/config/platConfig'
import type { IPlatformInteraction, ... } from '../types'
import { getHomeFeedList, homeFeedCursor } from './homeFeed'

class KuaishouPlatformInteraction implements IPlatformInteraction {
 readonly platformType = PlatType.KWAI

 resetHomeFeedCursor(): void {
 homeFeedCursor.reset()
 }

 async likeWork(workId: string, isLike: boolean): Promise<LikeResult> {
 // Implementation
 }

 async commentWork(params: CommentParams): Promise<CommentResult> {
 // Implementation
 }

 async favoriteWork(workId: string, isFavorite: boolean): Promise<FavoriteResult> {
 // Implementation
 }

 async getHomeFeedList(params: HomeFeedListParams): Promise<HomeFeedListResult> {
 return getHomeFeedList(params)
 }
}

export const kuaishouInteraction = new KuaishouPlatformInteraction()

// Type
export type { KuaishouHomeFeedItem, KuaishouHomeFeedResponse } from './types'
```

### 5:Type

```typescript
// plats/types.ts
export type SupportedPlatformType = PlatType.Xhs | PlatType.Douyin | PlatType.KWAI
```

### 6:

```typescript
// plats/manager.ts
import { kuaishouInteraction } from './kuaishou'

constructor() {
 this.register(xhsInteraction)
 this.register(douyinInteraction)
 this.register(kuaishouInteraction) // New
}
```

### 7:

```typescript
// plats/index.ts
export { kuaishouInteraction } from './kuaishou'
export type { KuaishouHomeFeedItem, KuaishouHomeFeedResponse } from './kuaishou'
```

## addFunctionmodule

add「」Function:

### 1: types.ts addType

```typescript
// plats/types.ts
export interface SearchParams {
 keyword: string
 page: number
 size: number
}

export interface SearchResult extends BaseResult {
 items: HomeFeedItem[]
 hasMore: boolean
}
```

### 2:interface

```typescript
// plats/types.ts
export interface IPlatformInteraction {
 // ... Method
 search?(params: SearchParams): Promise<SearchResult>
}
```

### 3:PlatformFunctionmodule

```typescript
// plats/xhs/search.ts
export async function search(params: SearchParams): Promise<SearchResult> {
 // Implementation
}
```

### 4:

```typescript
// plats/xhs/index.ts
import { search } from './search'

class XhsPlatformInteraction {
 async search(params: SearchParams): Promise<SearchResult> {
 return search(params)
 }
}
```

## Implementation

use,Implementation:

1. `XxxInteractionService.ts`
2. `homeInject/types.ts` addType
3. `homeInject/constants.ts` add Action
4. `homeInject/WebAPI.ts` Method
5. `content_script_home.tsx`
6. `background.ts` `HomeInjectBackgroundHandler.ts`
