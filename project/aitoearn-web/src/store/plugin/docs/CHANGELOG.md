#

## v2.0.0 (2025-12-02)

### ✨ Function

#### 1. Tasks

- **Tasks**: use zustand persist ,Tasks localStorage
- **Tasks**:
 - PlatformTasks(Platform)
 - PlatformTasks
 - Tasks
- **Tasks**:
 - Tasks(100)
 - Tasks
 -

#### 2. (PublishListModal)

- Tasks
- Tasks(,,,Fail)
- Platform
-
-
- ,mobile

#### 3. (PublishDetailModal)

- Tasks(,Description,Created time)
- Platform
- ( + )
- (ID,)
- Wrong
- use:
 - task
 - taskId( store )
-
-

#### 4. Internationalization

- ** (zh-CN)**:
- ** (en)**:
- **Internationalization**:
 -
 -
 - Wrong
 -
 -

#### 5. New Store Method

```typescript
// addTasks
addPublishTask(task): string

// PlatformTasks
updatePlatformTask(taskId, platform, updates): void

// Tasks
deletePublishTask(taskId): void

// Tasks
clearPublishTasks(): void

// Tasks
getPublishTask(taskId): PublishTask | undefined

// Tasks
updateTaskListConfig(config): void
```

### 📦 New

```
src/store/plugin/
├── locales/ # Internationalization
│ ├── zh-CN.json #
│ └── en.json #
├── types/
│ └── publishTask.baseTypes.ts # TasksType
├── components/ #
│ ├── PublishListModal.tsx #
│ ├── PublishListModal.module.scss #
│ ├── PublishDetailModal.tsx #
│ ├── PublishDetailModal.module.scss #
│ ├── index.ts #
│ ├── example.tsx # use
│ └── README.md # component documentation
├── QUICK_START.md #
└── CHANGELOG.md #
```

### 🔄

- **store.ts**: Tasks ExtendedPluginStore
- **baseTypes.ts**: addTasksType
- **constants.ts**: addInternationalization key
- **index.ts**: NewType

### 🎨

- use CSS ,
- BEM namingspecification(Modify)
- SCSS Module module
-

### 📚

- ✅ use (`components/README.md`)
- ✅ (`QUICK_START.md`)
- ✅ (`components/example.tsx`)
- ✅ TypeDescription
- ✅ InternationalizationDescription

### 🔧

- **zustand**:
- **zustand/middleware**: persist (Tasks)
- **react-i18next**: Internationalization
- **ant-design**: UI
- **dayjs**:
- **TypeScript**: Type

### 💡 useScenario

1. **Platform**: Douyin,XiaohongshuPlatform
2. ****: Platform
3. **records**: records
4. ****: Platform

### 🚀

```tsx
import {
 usePluginStore,
 PublishListModal,
 PublishDetailModal,
 PlatformTaskStatus,
} from '@/store/plugin'

// Tasks
const taskId = addPublishTask({
 title: 'Video',
 platformTasks: [...]
})

//
updatePlatformTask(taskId, 'douyin', {
 status: PlatformTaskStatus.PUBLISHING,
 progress: { stage: 'upload', progress: 50 }
})

//
<PublishListModal visible={true} />
<PublishDetailModal taskId={taskId} />
```

### 📝

#### v1.x

v2.0 v1.x ,Modify.

FunctionFunction:

- useTasksFunction, v1.x
- useFunction

### ⚠️ Notes

1. **localStorage **: Tasks localStorage,
2. **Tasks**: 100 Tasks,
3. **Internationalization**: react-i18next
4. **Type**: Store MethodMethod,useCheck

### 🎯

- [ ] Tasks(JSON,CSV)
- [ ] Tasks
- [ ] addTasks
- [ ] Tasks
- [ ] addTasksNotesFunction

---

****: [README.md](./README.md)
****: [QUICK_START.md](./QUICK_START.md)
**component documentation**: [components/README.md](./components/README.md)
