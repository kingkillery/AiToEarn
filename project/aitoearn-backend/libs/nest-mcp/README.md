# @yikart/aitoearn-queue

A unified queue management module built on BullMQ.

## Features

- Unified queue service (`QueueService`)
- Centralized queue name management (`QueueName` enum)
- Built-in Redis connection management
- Type-safe job payload definitions

## Usage

### Import the module in your application

```typescript
import { AitoearnQueueModule } from '../src/queue.module'

@Module({
  imports: [
    AitoearnQueueModule.forRoot({
      redis: {
        host: 'localhost',
        port: 6379,
      },
      prefix: '{bull}',
    }),
  ],
})
export class AppModule {}
```

### Use the queue service

```typescript
import { QueueService } from '../src/queue.module'

@Injectable()
export class MyService {
  constructor(private readonly queueService: QueueService) {}

  async addJob() {
    await this.queueService.addMaterialGenerateJobs([{ taskId: '123' }])
  }
}
```

## Notes

- Do not use the `@InjectQueue` decorator directly; use `QueueService` instead
- Do not import types directly from `bullmq`; this module already provides required interfaces
- Consumers (queue processors) are still kept in each application
