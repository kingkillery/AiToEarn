# Components

.,.

##

| / | Description |
| ----------------- | ------------------------------------------- |
| `ui/` | shadcn/ui base component |
| `common/` | Function |
| `modals/` | Modal |
| `Plugin/` | |
| `Home/` | (AgentGenerator,HomeChat ) |
| `Chat/` | (ChatInput,ChatMessage ) |
| `PublishDialog/` | Dialogue(PC/mobile) |
| `ChannelManager/` | Modal |
| `SettingsModal/` | |
| `notification/` | |

---

## ui/ - shadcn/ui base component

Newuse `npx shadcn@latest add <component>` .

| | Description |
| ----------------------- | ------------------------------------------------------------------------------------------- |
| `modal.tsx` | Modal ( dialog), antd Modal |
| `searchable-select.tsx` | ( command + popover) |
| `form.tsx` | ( react-hook-form, FormField/FormItem/FormLabel/FormControl/FormMessage) |
| `star-rating.tsx` | (,) |
| `sonner.tsx` | Toast ( `@/lib/toast` use) |
| `number-input.tsx` | ( react-number-format, type="number") |

> **Modal** use:`<Modal open onClose title="" footer={...}>`, `@/lib/confirm`.use antd Modal.

---

## common/ - Function

| | Description |
| ------------------ | ------------------------------------------ |
| `AppReleaseModal` | , Providers |
| `DownloadAppModal` | App Modal |
| `LanguageSwitcher` | |
| `MediaPreview` | Media preview(/Video/), |
| `GlobalLoginModal` | Modal() |
| `LoginModal` | Modal(Form page/Scenario) |
| `FavoriteButton` | , loading |
| `EditTitleModal` | Modal, |

---

## modals/ - Modal

| | Description |
| ----------------------------- | ------------ |
| `VipContentModal` | VIP |
| `SubscriptionManagementModal` | |
| `PointsDetailModal` | Credits |
| `PointsRechargeModal` | CreditsValue |

---

## Plugin/ -

| | Description |
| -------------------- | ----------------------------------------------------------------- |
| `PluginModal` | (->/->/->Tab ) |
| `PublishDetailModal` | Tasks |

:`PluginNotInstalled`,`PluginNoPermission`,`PluginReady`( AccountsTab,PublishListTab)

---

## Home/ -

| | Description |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| `AgentGenerator` | AI (SSE Dialogue,,Platform). [AgentGenerator/README.md](./Home/AgentGenerator/README.md) |
| `PromptGallery` | (,,) |
| `HomeChat` | ,Dialogue |
| `TaskPreview` | Tasks |
| `Footer` | (,,) |

---

## Chat/ -

| | Description |
| ------------- | --------------------------------- |
| `ChatInput` | , |
| `ChatMessage` | (user/assistant) |
| `MediaUpload` | , |
| `TaskCard` | Tasks( `TaskCardSkeleton`) |

---

##

| | Description |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| `AvatarPlat` | Platform |
| `AvatarCropModal` | Modal( cropperjs, 400x400 PNG) |
| `WalletAccountSelect` | () |
| `SignInCalendar` | |
| `ScrollButtonContainer` | |
| `ChooseAccountModule` | module |
| `GetCode` | |
| `UserLogsModal` | AI useModal() |
| `VideoHistoryModal` | VideoModal(,,) |
| `InviteCodeHandler` | (, Providers,) |
| `RewardDisplay` | Tasks,PricingType(/CPM/CPE), sidebar/card/compact |

### Description

**PublishDialog** - Dialogue()

- PC :`PublishDialog/index.tsx`(Function)
- mobile:`PublishDialog/compoents/mobile/MobilePublishContent.tsx`()
- **ModifyCheck！**
- [PublishDialog/README.md](./PublishDialog/README.md)

**ChannelManager** - Modal

- /, `useChannelManagerStore` `setOpen(true)`

**NotificationPanel** -

- + ,
- :`useNotificationStore`(`src/store/notification.ts`)
- `NotificationControlModal`

---

## Newspecification

1. this document,
2. New**this document**
3. naming **PascalCase**,use(`index.tsx` + `*.module.scss`)
4. use **shadcn/ui**,use antd
