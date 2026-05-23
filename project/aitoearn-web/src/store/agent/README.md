# Agent Store

 AI Agent Tasksmodule.

## Directory structure

```text
src/store/agent/
├── index.ts # , store
├── agent.types.ts # Type
├── agent.constants.ts #
├── agent.state.ts #
├── agent.methods.ts # Method
├── handlers/ #
│ ├── index.ts #
│ └── action.handlers.ts # Action
├── utils/ # utility functions
│ ├── index.ts #
│ ├── refs.ts # Refs
│ ├── message.ts #
│ └── progress.ts #
├── task-instance/ # Tasks
│ ├── index.ts # module
│ ├── task-instance.types.ts # Type
│ ├── TaskInstance.ts # ()
│ ├── message.handler.ts #
│ ├── workflow.handler.ts #
│ └── sse.handler.ts # SSE
└── README.md # this document
```

## moduleDescription

### index.ts -

 store module:

```typescript
import { useAgentStore } from '@/store/agent'

const { isGenerating, messages, createTask, stopTask } = useAgentStore()
```

### handlers/ -

#### Action (action.handlers.ts)

useTasks:

```typescript
import { ActionRegistry } from '@/store/agent'

// Action
ActionRegistry.register({
 type: 'customAction',
 canHandle: (taskData) => taskData.action === 'customAction',
 execute: async (taskData, context) => {
 //
 },
})
```

 Action:

- `navigateToPublish` -
- `navigateToDraft` -
- `saveDraft` -
- `updateChannel` -
- `loginChannel` -

### utils/ -

#### refs.ts - Refs

,:

```typescript
import { createAgentRefs, resetAgentRefs } from '@/store/agent'

const refs = createAgentRefs()
resetAgentRefs(refs) // refs
```

#### message.ts -

:

```typescript
import { createMessageUtils } from '@/store/agent'

const messageUtils = createMessageUtils({ refs, set, get })
const userMsg = messageUtils.createUserMessage('Hello', [])
messageUtils.markMessageDone()
```

#### progress.ts -

(use).

### task-instance/ - Tasks

TaskInstance SolutionTasks. Agent Tasks TaskInstance ,.

#### modulestructure

| | | |
| ------------------------ | ---- | ---------------------------------- |
| `task-instance.types.ts` | ~100 | Type(interface,interface) |
| `message.handler.ts` | ~250 | (,,) |
| `workflow.handler.ts` | ~260 | (,) |
| `sse.handler.ts` | ~290 | SSE |
| `TaskInstance.ts` | ~415 | (,module) |

#### TaskInstance.ts -

**:**

- **instanceId**: (,)
- **taskId**: TasksID(IDID)
- ** refs**: `currentAssistantMessageId`,`streamingText`,`currentStepWorkflow`
- **SSE **: SSE TaskInstance,Tasks
- ****: TaskInstance , handler module

#### message.handler.ts -

Function:

```typescript
//
createUserMessage(content, medias?) //
createAssistantMessage(ctx) // AI

//
markMessageDone(ctx) //
markMessageError(ctx, error) // Wrong
updateMessageContent(ctx, content) //
updateMessageWithActions(ctx, content, actions) // actions
```

#### workflow.handler.ts -

:

```typescript
startNewStep(ctx) //
addWorkflowStep(ctx, step) // add
updateLastWorkflowStep(ctx, updater) //
handleToolCallComplete(ctx, name, input) //
handleToolResult(ctx, resultText) //
```

#### sse.handler.ts - SSE

 SSE :

```typescript
handleSSEMessage(ctx, msg, callbacks?) // SSE
// : stream_event, assistant, user, result, error, done
```

**use:**

```typescript
import { TaskInstance, getTaskInstance, getOrCreateTaskInstance } from '@/store/agent'

// Tasks
const instanceContext: ITaskInstanceContext = {
 syncToStore: (taskId, updater) => updateTaskData(taskId, updater),
 getData: (taskId) => getTaskData(taskId),
 migrateTaskData: (fromTaskId, toTaskId) => {
 /* */
 },
 setCurrentTaskId: (taskId) => set({ currentTaskId: taskId }),
}

// Tasks
const instance = new TaskInstance(tempTaskId, instanceContext)

// Action
instance.setTranslation(t)
instance.setActionContext(actionContext)

// add
const userMessage = instance.createUserMessage(prompt, medias)
instance.addMessage(userMessage)

// SSE
instance.handleSSEMessage(sseMessage, callbacks)

// Tasks
const instance = getOrCreateTaskInstance(taskId, context)

//
const existing = getTaskInstance(taskId)
```

** API:**

| Method | Description |
| --------------------------------------- | -------------------------------- |
| `createUserMessage(content, medias?)` | |
| `createAssistantMessage()` | AI |
| `addMessage(message)` | addTasks |
| `handleSSEMessage(message, callbacks?)` | SSE |
| `markMessageDone()` | |
| `markMessageError(error)` | Wrong |
| `setIsGenerating(value)` | |
| `setProgress(value)` | |
| `migrateToRealTaskId(newTaskId)` | TasksID(IDID) |
| `resetForNewRound()` | (Dialogue) |
| `abort()` | SSE |

## use

### use

```tsx
import { useAgentStore } from '@/store/agent'
import { useShallow } from 'zustand/react/shallow'

function ChatPage() {
 const { messages, isGenerating, createTask, setActionContext } = useAgentStore(
 useShallow((state) => ({
 messages: state.messages,
 isGenerating: state.isGenerating,
 createTask: state.createTask,
 setActionContext: state.setActionContext,
 }))
 )

 const router = useRouter()
 const { t } = useTranslation()

 // action
 useEffect(() => {
 setActionContext({ router, lng: 'zh-CN', t })
 }, [router, t])

 const handleSend = async (prompt: string) => {
 await createTask({ prompt, t })
 }

 return <div>{/* ... */}</div>
}
```

### Dialogue

```tsx
const { continueTask } = useAgentStore()

const handleContinue = async (prompt: string, taskId: string) => {
 await continueTask({ prompt, taskId, t })
}
```

##

### add Action

1. `handlers/action.handlers.ts` add:

```typescript
const customActionHandler: IActionHandler = {
 type: 'customAction',
 canHandle: (taskData) => taskData.action === 'customAction',
 execute: async (taskData, context) => {
 //
 },
}
```

2. add:

```typescript
ActionRegistry.register(customActionHandler)
```

### addutility functions

 `utils/` , `utils/index.ts` .

## Notes

1. ** ID **:use ID ,Wrong.
2. **Refs vs State**:use ref,.
3. **Action **:use action `setActionContext`.
4. ****:Xiaohongshu/Douyin.

## ⚠️ : `this`

### Description

use( `createStoreMethods`),Methoduse `this` Method,Method,`this` .

### Wrong

```typescript
// ❌ Wrong:use this
function createMethods() {
 return {
 methodA() {
 console.log('A')
 },
 methodB() {
 // methodB ,this
 this.methodA() // TypeError: Cannot read properties of undefined
 },
 }
}

// useScenario()
const methods = createMethods()
someAsyncFunction((data) => {
 methods.methodB() //
})

//
someAsyncFunction(methods.methodB) // this ！
```

### CorrectApproach

Method,:

```typescript
// ✅ Correct:use
function createMethods() {
 //
 function methodA() {
 console.log('A')
 }

 function methodB() {
 // , this
 methodA()
 }

 // Method
 return {
 methodA,
 methodB,
 }
}
```

### Check

Method,Check:

1. Method？
2. MethodMethod？
3. ,use `this`？

,use `this`.
