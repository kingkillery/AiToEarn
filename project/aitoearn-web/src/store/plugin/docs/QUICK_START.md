# - Tasks

## 🚀 5

### 1.

```bash
pnpm install dayjs
```

### 2. Hook

```tsx
import {
 usePluginStore,
 PublishListModal,
 PublishDetailModal,
 PlatformTaskStatus,
 type PublishTask,
} from '@/store/plugin'
```

### 3.

```tsx
'use client'

import { useState } from 'react'
import { Button } from 'antd'
import {
 usePluginStore,
 PublishListModal,
 PublishDetailModal,
 PlatformTaskStatus,
 type PublishTask,
} from '@/store/plugin'

export default function PublishPage() {
 const [listVisible, setListVisible] = useState(false)
 const [detailVisible, setDetailVisible] = useState(false)
 const [selectedTask, setSelectedTask] = useState<PublishTask>()

 const { addPublishTask, updatePlatformTask } = usePluginStore()

 // Tasks
 const handlePublish = async () => {
 // 1. Tasks
 const taskId = addPublishTask!({
 title: 'Video',
 description: 'DouyinXiaohongshu',
 platformTasks: [
 {
 platform: 'douyin',
 params: {
 platform: 'douyin',
 type: 'video',
 title: 'Video',
 desc: 'VideoDescription',
 // video: videoFile,
 // cover: coverFile,
 },
 status: PlatformTaskStatus.PENDING,
 progress: null,
 result: null,
 startTime: null,
 endTime: null,
 error: null,
 },
 {
 platform: 'xhs',
 params: {
 platform: 'xhs',
 type: 'video',
 title: 'Video',
 desc: 'VideoDescription',
 },
 status: PlatformTaskStatus.PENDING,
 progress: null,
 result: null,
 startTime: null,
 endTime: null,
 error: null,
 },
 ],
 })

 // 2.
 await publishToPlatforms(taskId)

 // 3.
 setSelectedTask(usePluginStore.getState().getPublishTask!(taskId))
 setDetailVisible(true)
 }

 // Platform
 const publishToPlatforms = async (taskId: string) => {
 // Douyin
 updatePlatformTask!(taskId, 'douyin', {
 status: PlatformTaskStatus.PUBLISHING,
 startTime: Date.now(),
 })

 //
 await simulatePublish(taskId, 'douyin')

 // Xiaohongshu
 updatePlatformTask!(taskId, 'xhs', {
 status: PlatformTaskStatus.PUBLISHING,
 startTime: Date.now(),
 })

 await simulatePublish(taskId, 'xhs')
 }

 //
 const simulatePublish = async (taskId: string, platform: 'douyin' | 'xhs') => {
 //
 for (let i = 0; i <= 100; i += 20) {
 await new Promise((resolve) => setTimeout(resolve, 500))
 updatePlatformTask!(taskId, platform, {
 progress: {
 stage: 'upload',
 progress: i,
 message: ` ${i}%`,
 timestamp: Date.now(),
 },
 })
 }

 //
 updatePlatformTask!(taskId, platform, {
 status: PlatformTaskStatus.COMPLETED,
 endTime: Date.now(),
 progress: {
 stage: 'complete',
 progress: 100,
 message: 'Success',
 timestamp: Date.now(),
 },
 result: {
 success: true,
 workId: `${platform}_${Date.now()}`,
 shareLink: `https://${platform}.com/video/${Date.now()}`,
 publishTime: Date.now(),
 },
 })
 }

 //
 const handleViewDetail = (task: PublishTask) => {
 setSelectedTask(task)
 setDetailVisible(true)
 setListVisible(false)
 }

 return (
 <div style={{ padding: 20 }}>
 <h1></h1>

 <div style={{ marginBottom: 20 }}>
 <Button type="primary" onClick={handlePublish}>

 </Button>{' '}
 <Button onClick={() => setListVisible(true)}></Button>
 </div>

 {/* */}
 <PublishListModal
 visible={listVisible}
 onClose={() => setListVisible(false)}
 onViewDetail={handleViewDetail}
 />

 {/* */}
 <PublishDetailModal
 visible={detailVisible}
 onClose={() => {
 setDetailVisible(false)
 setSelectedTask(undefined)
 }}
 task={selectedTask}
 />
 </div>
 )
}
```

## 📦

### 1. Tasks (PublishTask)

,Platform.

```typescript
{
 id: 'task_123',
 title: 'Video',
 platformTasks: [
 { platform: 'douyin', ... },
 { platform: 'xhs', ... },
 ],
 overallStatus: 'publishing',
 ...
}
```

### 2. PlatformTasks (PlatformPublishTask)

PlatformTasks.

```typescript
{
 platform: 'douyin',
 status: 'publishing',
 progress: {
 stage: 'upload',
 progress: 45,
 message: '...',
 },
 result: null,
 ...
}
```

### 3. Tasks (PlatformTaskStatus)

- `PENDING` -
- `PUBLISHING` -
- `COMPLETED` -
- `ERROR` - Fail

## 🔄

```
1. Tasks (addPublishTask)
 ↓
2. PUBLISHING (updatePlatformTask)
 ↓
3. (updatePlatformTask)
 ↓
4. (updatePlatformTask)
 ↓
5. (PublishDetailModal)
```

## 🎯

### 1. Wrong

```tsx
try {
 await publishToPlatform(taskId, 'douyin')
} catch (error) {
 updatePlatformTask!(taskId, 'douyin', {
 status: PlatformTaskStatus.ERROR,
 error: error.message,
 endTime: Date.now(),
 })
}
```

### 2.

```tsx
const publishWithProgress = async (taskId, platform) => {
 updatePlatformTask!(taskId, platform, {
 status: PlatformTaskStatus.PUBLISHING,
 startTime: Date.now(),
 })

 // use publish Method,
 await window.AIToEarnPlugin.publish(params, (progress) => {
 updatePlatformTask!(taskId, platform, {
 progress,
 })
 })
}
```

### 3.

```tsx
const publishToMultiplePlatforms = async (taskId, platforms) => {
 //
 await Promise.all(platforms.map((platform) => publishToPlatform(taskId, platform)))
}
```

## 🌍 Internationalization

Internationalization, `react-i18next`.

```tsx
import { useTranslation } from 'react-i18next'

const { t } = useTranslation()
t('plugin.publishList.title') // ""
```

## 📱

mobile,.

## 🔧 Tasks

```tsx
const { updateTaskListConfig } = usePluginStore()

// Tasks
updateTaskListConfig!({
 maxTasks: 50, // 50Tasks
 autoCleanCompleted: true, // Tasks
 cleanAfter: 7 * 24 * 60 * 60 * 1000, // 7
})
```

## 📚

- [](./components/README.md)
- [](./components/example.tsx)
- [API ](./README.md)

## 💡

### Q: Tasks？

A: Tasks zustand persist localStorage.

### Q: Tasks？

```tsx
const { clearPublishTasks } = usePluginStore()
clearPublishTasks!()
```

### Q: Tasks？

```tsx
const { deletePublishTask } = usePluginStore()
deletePublishTask!('task_id')
```

## 🎉 ！

Tasks,Function！
