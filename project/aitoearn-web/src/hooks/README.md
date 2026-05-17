# Hooks

 React Hooks.

##

| | Description |
| --------------------- | ----------------------------------------- |
| `useDocumentTitle.ts` | Hook |
| `useMediaUpload.ts` | Hook,, |
| `useNotification.ts` | Hook |
| `useSystem.ts` | Hook |

---

## useDocumentTitle - Hook

(`document.title`).

###

```typescript
import { useDocumentTitle } from '@/hooks'
//
import { useDocumentTitle } from '@/hooks/useDocumentTitle'
```

### API

```typescript
function useDocumentTitle(title: string | undefined | null, defaultTitle?: string): void
```

| parameters | Type | Description |
| -------------- | ----------------------------- | ----------------------------------- |
| `title` | `string \| undefined \| null` | (use defaultTitle) |
| `defaultTitle` | `string` | (title use) |

### use

```tsx
import { useDocumentTitle } from '@/hooks'

function ChatPage() {
 const { t } = useTransClient('chat')
 const [task, setTask] = useState<Task | null>(null)

 //
 useDocumentTitle(task?.title, t('task.newChat'))

 return <div>...</div>
}
```

###

- ****: `{title} —— AiToEarn`
- **Value**: title use defaultTitle
- ****:title

---

## useMediaUpload - Hook

,,,.

###

```typescript
import { useMediaUpload } from '@/hooks'
//
import { useMediaUpload } from '@/hooks/useMediaUpload'
```

### API

```typescript
interface IUseMediaUploadOptions {
 /** Fail */
 onError?: (error: Error) => void
}

interface IUseMediaUploadReturn {
 /** */
 medias: IUploadedMedia[]
 /** */
 setMedias: React.Dispatch<React.SetStateAction<IUploadedMedia[]>>
 /** */
 isUploading: boolean
 /** () */
 handleMediasChange: (files: FileList) => Promise<void>
 /** */
 handleMediaRemove: (index: number) => void
 /** */
 cancelUpload: () => void
 /** */
 clearMedias: () => void
}
```

### use

```tsx
import { useMediaUpload } from '@/hooks'
import { toast } from '@/lib/toast'

function MyComponent() {
 const { medias, isUploading, handleMediasChange, handleMediaRemove, clearMedias } =
 useMediaUpload({
 onError: (error) => toast.error('Fail: ' + error.message),
 })

 const handleSubmit = async () => {
 // ...
 clearMedias() //
 }

 return (
 <div>
 {/* Media preview */}
 {medias.map((media, index) => (
 <div key={index}>
 <img src={media.url} alt="" />
 <button onClick={() => handleMediaRemove(index)}></button>
 </div>
 ))}

 {/* */}
 <input
 type="file"
 accept="image/*,video/*"
 multiple
 onChange={(e) => e.target.files && handleMediasChange(e.target.files)}
 disabled={isUploading}
 />
 </div>
 )
}
```

###

- **Path URL**: S3 URL( `https://aitoearn.ap-southeast-1.amazonaws.com/userId/hash/filename.jpg`)
- ****:
- ****:
- ****:

### Notes

- `media.url` S3 URL(`presignedData.url + presignedData.fields.key`)
- `media.progress` 0-100 , `undefined`
- Fail `onError`

---

## New Hook specification

add Hook ,:

1. Checkthis documentalready existsFunction
2. Hook()
3. add JSDoc
4. this document
5. `index.ts` Hook
