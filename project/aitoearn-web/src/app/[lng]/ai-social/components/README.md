# Home specification

## specification

###

moduleuse,:

```tsx
<section className="py-8 px-4 md:px-6 lg:px-8">
 <div className="w-full max-w-5xl mx-auto">{/* */}</div>
</section>
```

**:**

- :`max-w-5xl`(1024px)
- :`mx-auto`
- :`px-4 md:px-6 lg:px-8`

###

- section :`py-8` `py-12`
- :`mb-6` `mb-8`

##

| | Path | Description |
| ------------- | ----------------- | ------------------------------- |
| HomeChat | `./HomeChat` | |
| AgentFeatures | `./AgentFeatures` | AI Agent Function() |
| PromptGallery | `./PromptGallery` | |
| TaskPreview | `./TaskPreview` | Tasks |

## Wrong

### ❌ Wrong

```tsx
//
<div className="max-w-6xl mx-auto"> // Wrong！ max-w-5xl
<div className="max-w-4xl mx-auto"> // Wrong！ max-w-5xl
```

### ✅ Correct

```tsx
<div className="w-full max-w-5xl mx-auto">
```

## specification

 `.claude/rules/default.md`:

- use shadcn/ui (`text-foreground`,`bg-muted` )
- use( `text-gray-900`,`bg-white`)
- use `var.css`
