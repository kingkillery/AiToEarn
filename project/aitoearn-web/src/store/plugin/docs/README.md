# module

## 📁 structure

```
src/store/plugin/
├── index.ts #
├── store.ts # Zustand Store
├── hooks.ts # custom Hooks
├── baseTypes.ts # TypeScript Type
├── constants.ts #
├── utils.ts # utility functions
├── README.md # this document
└── examples/ # use
 ├── basic.example.tsx
 └── advanced.example.tsx
```

## 🚀

### 1. use

```typescript
import { usePlugin } from '@/store/plugin';

export function MyComponent() {
 // ,2
 const { isConnected, status } = usePlugin(true, 2000);

 return (
 <div>
 : {isConnected ? '' : ''}
 </div>
 );
}
```

### 2. Function

```typescript
import { usePluginLogin } from '@/store/plugin';

export function LoginButton() {
 const { login } = usePluginLogin();

 const handleLogin = async () => {
 const result = await login('douyin');
 if (result.success) {
 console.log('Success:', result.data?.nickname);
 }
 };

 return <button onClick={handleLogin}>Douyin</button>;
}
```

### 3. Video

```typescript
import { usePluginPublish } from '@/store/plugin';

export function PublishVideo() {
 const { publishVideo, isPublishing, publishProgress } = usePluginPublish();

 const handlePublish = async (videoFile: File, coverFile: File) => {
 const result = await publishVideo(
 'douyin',
 videoFile,
 coverFile,
 { title: 'Video', desc: 'Description' },
 (progress) => console.log(`: ${progress.progress}%`)
 );

 if (result.success) {
 alert('Success！');
 }
 };

 return (
 <div>
 <button onClick={() => handlePublish(...)} disabled={isPublishing}>
 {isPublishing ? '...' : 'Video'}
 </button>
 {publishProgress && <div>: {publishProgress.progress}%</div>}
 </div>
 );
}
```

### 4.

```typescript
import { usePluginWorkflow } from '@/store/plugin';

export function OneClickPublish() {
 const { loginAndPublishVideo } = usePluginWorkflow();

 const handleOneClick = async (videoFile: File, coverFile: File) => {
 // +
 const result = await loginAndPublishVideo(
 'douyin',
 videoFile,
 coverFile,
 { title: '' }
 );

 if (result.success) {
 alert('Success！');
 }
 };

 return <button onClick={() => handleOneClick(...)}></button>;
}
```

## 📚 API

### Hooks

#### `usePlugin(autoPolling?, pollingInterval?)`

Method

**parameters:**

- `autoPolling` - , `true`
- `pollingInterval` - (), `2000`

**:**

```typescript
{
 status: PluginStatus;
 isConnected: boolean;
 isNotInstalled: boolean;
 isChecking: boolean;
 isPublishing: boolean;
 publishProgress: ProgressEvent | null;
 checkPlugin: () => boolean;
 startPolling: (interval?: number) => void;
 stopPolling: () => void;
 login: (platform: PlatformType) => Promise<PlatAccountInfo>;
 publish: (params: PublishParams, onProgress?) => Promise<PublishResult>;
 resetPublishState: () => void;
}
```

#### `usePluginLogin()`

Function

**:**

```typescript
{
 login: (platform: PlatformType) => Promise<OperationResult<PlatAccountInfo>>
}
```

#### `usePluginPublish()`

Function

**:**

```typescript
{
 publish: (params: PublishParams, onProgress?) => Promise<OperationResult>;
 publishVideo: (platform, video, cover, options?, onProgress?) => Promise<OperationResult>;
 publishImages: (platform, images, options?, onProgress?) => Promise<OperationResult>;
 isPublishing: boolean;
 publishProgress: ProgressEvent | null;
 resetPublishState: () => void;
}
```

#### `usePluginWorkflow()`

(+)

**:**

```typescript
{
 isConnected: boolean;
 loginAndPublishVideo: (...) => Promise<OperationResult>;
 loginAndPublishImages: (...) => Promise<OperationResult>;
}
```

### utility functions

```typescript
//
getPluginStatusText(status: PluginStatus): string;
getPublishStageText(stage: ProgressEvent['stage']): string;

//
isPluginConnected(status: PluginStatus): boolean;
isPluginNotInstalled(status: PluginStatus): boolean;

//
formatProgress(progress: number): string;
formatFileSize(bytes: number): string;

//
validateFileType(file: File, acceptTypes: string[]): boolean;
isValidVideoFile(file: File): boolean;
isValidImageFile(file: File): boolean;
validateFileSize(file: File, maxSize: number): boolean;

//
withRetry<T>(fn: () => Promise<T>, maxRetries?, delay?): () => Promise<T>;
```

### Type

```typescript
//
enum PluginStatus {
 UNKNOWN = 'UNKNOWN',
 CHECKING = 'CHECKING',
 CONNECTED = 'CONNECTED',
 NOT_INSTALLED = 'NOT_INSTALLED',
}

// PlatformType
type PlatformType = 'douyin' | 'xhs' | 'kwai' | 'bilibili'

// parameters
interface PublishParams {
 platform: PlatformType
 type: 'video' | 'image'
 title?: string
 desc?: string
 video?: File | string
 cover?: File | string
 images?: (File | string)[]
 topics?: string[]
 visibility?: 'public' | 'private' | 'friends'
 // ...Field
}

//
interface OperationResult<T = any> {
 success: boolean
 data?: T
 error?: string
}
```

###

```typescript
// (2)
DEFAULT_POLLING_INTERVAL = 2000

//
PLUGIN_STATUS_TEXT = {
 UNKNOWN: '',
 CHECKING: '...',
 CONNECTED: '',
 NOT_INSTALLED: '',
}

//
PUBLISH_STAGE_TEXT = {
 download: '',
 upload: '',
 publish: '',
 complete: '',
 error: 'Wrong',
}

// Wrong
ERROR_MESSAGES = {
 PLUGIN_NOT_INSTALLED: ' Aitoearn ',
 PUBLISHING_IN_PROGRESS: ',',
 LOGIN_FAILED: 'Fail',
 PUBLISH_FAILED: 'Fail',
}
```

## 💡 use

### 1:

```typescript
import { usePlugin, getPluginStatusText } from '@/store/plugin';

export function PluginStatus() {
 const { status } = usePlugin();

 return (
 <div>
 : {getPluginStatusText(status)}
 </div>
 );
}
```

### 2:

```typescript
import { isValidVideoFile, validateFileSize } from '@/store/plugin'

function handleFileSelect(file: File) {
 if (!isValidVideoFile(file)) {
 alert('Video')
 return
 }

 if (!validateFileSize(file, 500 * 1024 * 1024)) {
 alert('Video 500MB')
 return
 }

 // ...
}
```

### 3:

```typescript
import { usePluginPublish, withRetry } from '@/store/plugin';

export function PublishWithRetry() {
 const { publish } = usePluginPublish();

 const handlePublish = async (params: PublishParams) => {
 // 3 , 2
 const publishWithRetry = withRetry(
 () => publish(params),
 3,
 2000
 );

 try {
 const result = await publishWithRetry();
 console.log('Success:', result);
 } catch (error) {
 console.error('Fail:', error);
 }
 };

 return <button onClick={() => handlePublish(...)}></button>;
}
```

### 4:

```typescript
import { usePluginPublish, formatProgress, getPublishStageText } from '@/store/plugin';
import { Progress } from 'antd';

export function PublishWithProgress() {
 const { publishVideo, publishProgress } = usePluginPublish();

 return (
 <div>
 {publishProgress && (
 <>
 <Progress percent={publishProgress.progress} />
 <p>
 {getPublishStageText(publishProgress.stage)}: {formatProgress(publishProgress.progress)}
 </p>
 </>
 )}
 </div>
 );
}
```

## 📝 Notes

1. ****
 - use `usePlugin` will automatically
 - will automatically
 - `stopPolling()`

2. **Wrong**
 - Hooks Methoduse `OperationResult`
 - `success`,`data` `error` Field
 - use try-catch

3. ****
 - Video: ≤ 500MB
 - : ≤ 10MB
 - Image-text: 9

4. ****
 - Tasks
 - `isPublishing` `true`

5. **Platform**
 - PlatformFunction
 - Platform

## 🔗

- [Web API ](../../../demo/docs/WEB_API.md)
- [Type](../../../demo/PublishType/)
- [](./examples/)

## 📞

,.

---

****: 1.0.0
****: 2025-12-01
