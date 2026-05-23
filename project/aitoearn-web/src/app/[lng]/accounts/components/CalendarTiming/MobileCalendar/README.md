# MobileCalendar mobile

## Overview

mobile, FullCalendar .
Week view/View switch,task list for selected date.

## Directory structure

```
MobileCalendar/
├── index.tsx # main component,manages state and composes subcomponents
├── MobileCalendarHeader.tsx # top toolbar(,View switch)
├── MobileWeekView.tsx # week view component
├── MobileMonthView.tsx # month view component
├── MobileDayRecords.tsx # task list for selected date
├── mobileCalendar.types.ts # Type
└── README.md # this document
```

## Description

### MobileCalendar (index.tsx)

main component,is responsible for:

- `selectedDate`(currently selected date)
- `viewType`('week' | 'month')
- compose subcomponents
- `onClickPub` ,

**Props:**
| property | Type | Description |
|------|------|------|
| onClickPub | `(date: string) => void` | addTasks |

### MobileCalendarHeader

top toolbar,Function:

- show current year/month(:YYYY/MM)
- click year/month to show month picker
- week/month switch button
- Today button

**Props:**
| property | Type | Description |
|------|------|------|
| currentDate | `Date` | currently displayed date |
| viewType | `'week' \| 'month'` | Type |
| onDateChange | `(date: Date) => void` | date change callback |
| onViewTypeChange | `(type: ViewType) => void` | view switch callback |
| onToday | `() => void` | Today button |

### MobileWeekView

week view component,Function:

- show weekday header row(Sun/Mon/Tue...)
- show 7 days of current week
- supports left/right swipe to switch weeks
- highlight selected date
- dates with data show dots

**Props:**
| property | Type | Description |
|------|------|------|
| currentDate | `Date` | base date of current week |
| selectedDate | `Date` | selected date |
| onDateSelect | `(date: Date) => void` | date select callback |
| onWeekChange | `(direction: 'prev' \| 'next') => void` | week switch callback |
| recordMap | `Map<string, PublishRecordItem[]>` | records |

### MobileMonthView

month view component,Function:

- compact month calendar grid
- dates with data show dots
- highlight selected date
- click date to switch selection

**Props:**
| property | Type | Description |
|------|------|------|
| currentDate | `Date` | base date of current month |
| selectedDate | `Date` | selected date |
| onDateSelect | `(date: Date) => void` | date select callback |
| recordMap | `Map<string, PublishRecordItem[]>` | records |

### MobileDayRecords

Tasks,Function:

- show all tasks for selected date
- use RecordCore records
- support loading skeleton
- empty state
- addTasks

**Props:**
| property | Type | Description |
|------|------|------|
| selectedDate | `Date` | selected date |
| records | `PublishRecordItem[]` | records |
| loading | `boolean` | loading state |
| onClickPub | `(date: string) => void` | addTasks |

## Data flow

```
useCalendarTiming.recordMap (existing store)
 ↓
MobileCalendar (get full-month data to show dots)
 ↓
 ┌────┴────────┐
 ↓ ↓
/Month view MobileDayRecords
(based on recordMap (based on selectedDate
 show dots for dates with data) filter and display task list)
```

**recordMap structure:**

```typescript
Map<string, PublishRecordItem[]>
// key: date string format 'YYYY-MM-DD'
// value: Daterecords
```

## Reused components/tools

| / | Description |
| ----------------- | ---------------------------- |
| useCalendarTiming | data fetching and state management store |
| RecordCore | Tasks(mobile) |
| useIsMobile | device detection hook(< 768px) |
| getDays | dayjs utility functions |
| Popover | shadcn/ui popover component |
| Button/Skeleton | shadcn/ui base component |

## specification

- use Tailwind CSS
- follow shadcn/ui
- selected date: `bg-(--primary-color) text-white`
- today date: `text-blue-500`(when not selected)
- dot: `bg-blue-500 w-1.5 h-1.5 rounded-full`
- past date: `text-muted-foreground`
- non-current-month date: `text-muted-foreground/40`

## Description

### Date selection

click any date -> `selectedDate` -> task list refreshes

### Week view swipe

- **Swipe left**: show next week
- **Swipe right**: show previous week
- **Implementation**: touch event listening,calculate swipe distance and direction(Value 50px)

### View switch

click switch button -> Week view ⇄ Month view

- default is week view
- use(CalendarDays / Grid3X3)

### Month jump

click top year/month -> Popover -> select month -> jump and fetch data

## Internationalization

translation keys are located in `account` naming `mobileCalendar` :

```json
{
 "mobileCalendar": {
 "weekView": "Week view",
 "monthView": "Month view",
 "tasks": "Tasks",
 "addTask": "addTasks",
 "noTasks": "DateTasks",
 "addFirstTask": "addTasks"
 }
}
```

## Notes

1. **Data fetching**: when switching month call `getPubRecord()` fetch new month data
2. **dot**: show only when data exists, hide when empty
3. **Function**: mobileFunction( RecordCore )
4. **Data sync**: PC Data sync, `useCalendarTiming` store
5. **device detection**: use `useIsMobile` hook,breakpoint is 768px

## Modifyrecords

| Date | Modify |
| ---------- | --------------------------------------------- |
| 2025-12-30 | ,ImplementationWeek view/View switch,Tasks |
