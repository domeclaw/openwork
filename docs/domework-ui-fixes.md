# Domework UI Fixes - STOP Button, Thinking Card, Message Sync

> Summary of UI fixes on branch `domework` for OpenWork app
> Date: 2026-05-12
> Main files: `apps/app/src/react-app/domains/session/surface/`

---

## Problems Fixed

### 1. STOP button persists after agent asks a question
- **Symptom:** Agent asks a question and waits for user response, but STOP button remains visible
- **Cause:** `chatStreaming` used `liveStatus.type === "busy"` which backend reports even after agent finishes

### 2. Thinking content not visible during streaming
- **Symptom:** While agent is reasoning, user cannot see thinking text unless "Show Thinking" is enabled in settings
- **Cause:** `shouldShowReasoning = showThinking` hid reasoning parts when developer mode was off

### 3. Agent questions disappear / not visible
- **Symptom:** Agent asks a question but user cannot see it until pressing STOP
- **Cause:** SSE sync incomplete + polling stopped too early because `activeRun` was reset when assistant message finished

---

## Changes by File

### session-surface.tsx

#### activeRun state + assistantFinished detection
```tsx
const [activeRun, setActiveRun] = useState(false);

const assistantFinished = useMemo(() => {
  const lastMsg = renderedMessages[renderedMessages.length - 1];
  if (!lastMsg || lastMsg.role !== "assistant") return false;
  const lastPart = lastMsg.parts[lastMsg.parts.length - 1];
  if (!lastPart) return false;
  if (lastPart.type === "dynamic-tool") return false;
  if ("state" in lastPart && lastPart.state === "streaming") return false;
  return true;
}, [renderedMessages]);
```

#### chatStreaming that does not depend directly on backend status
```tsx
const chatStreaming = sending || (!assistantFinished && (liveStatus.type === "busy" || liveStatus.type === "retry"));
```

- `sending = true` -> STOP shows (user sent a message)
- `assistantFinished = false` -> STOP shows (agent still working)
- `assistantFinished = true` -> STOP hides (agent finished, regardless of backend status)

#### Grace period 3 seconds for polling
```tsx
useEffect(() => {
  if ((liveStatus.type === "idle" || currentSnapshot?.status?.type === "idle") && assistantFinished) {
    const timer = setTimeout(() => {
      setSending(false);
      setActiveRun(false);
    }, 3000);
    return () => clearTimeout(timer);
  }
}, [liveStatus.type, currentSnapshot?.status?.type, assistantFinished]);
```

- `activeRun` stays `true` for 3 more seconds after assistant finishes
- Polling refetch snapshot every 2 seconds continues to catch messages SSE may miss

#### Polling while activeRun = true
```tsx
useEffect(() => {
  if (!activeRun) return;
  const interval = setInterval(() => {
    void snapshotQuery.refetch().then((result) => {
      // ... reset if new assistant message found
    });
  }, 2000);
  return () => clearInterval(interval);
}, [activeRun, snapshotQuery]);
```

#### handleSend sets activeRun = true
```tsx
setSending(true);
setActiveRun(true);
```

#### handleAbort (press STOP)
```tsx
await abortSessionSafe(opencodeClient, props.sessionId);
await snapshotQuery.refetch();
```

---

### message-list.tsx

#### Thinking card with max-h + scroll
```tsx
if ((group.part as { _reasoning?: boolean })._reasoning) {
  const raw = partToText(group.part);
  if (!raw.trim()) return null;
  return (
    <details className="mb-2 rounded-lg border border-gray-6/20 bg-gray-1/20" open>
      <summary className="flex cursor-pointer items-center gap-1.5 px-3 py-2 text-[11px] font-medium uppercase tracking-wider text-gray-7 hover:bg-gray-2/40">
        <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2">
          <circle cx="12" cy="12" r="10" />
          <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3" />
          <line x1="12" y1="17" x2="12.01" y2="17" />
        </svg>
        Thinking
      </summary>
      <div className="max-h-[220px] overflow-y-auto border-t border-gray-6/10 px-3 py-2 font-mono text-[11px] leading-[1.5] whitespace-pre-wrap text-gray-10">
        {cleanReasoningPreview(raw)}
      </div>
    </details>
  );
}
```

- `open` - always expanded during streaming
- `max-h-[220px] overflow-y-auto` - limits height, does not push other content off-screen

#### shouldShowReasoning auto-shows during streaming
```tsx
const shouldShowReasoning = showThinking || props.isStreaming;
```

- `showThinking = true` (developer mode) - always show
- `isStreaming = true` (agent working) - temporarily show
- No need to open settings to see thinking during streaming

---

### composer.tsx

#### Dual buttons: Stop + Run task
```tsx
<div className="ml-auto flex shrink-0 items-end gap-2">
  {props.busy ? (
    <>
      <button type="button" onClick={props.onStop} className="...">
        <Square size={12} fill="currentColor" />
        <span>Stop</span>
      </button>
      {canSend && (
        <button type="button" onClick={props.onSend} className="...">
          <ArrowUp size={15} />
          <span>Run task</span>
        </button>
      )}
    </>
  ) : (
    <button type="button" onClick={props.onSend} ...>
      <ArrowUp size={15} />
      <span>Run task</span>
    </button>
  )}
</div>
```

- When busy: shows STOP button (red) + Run task button (if draft exists)
- When idle: shows normal Run task button

---

## Behavior After Fix

| Scenario | STOP Button | Thinking | Polling |
|----------|-------------|----------|---------|
| User sends message | **Shows** | - | Running |
| Assistant generating | **Shows** | Visible | Running |
| Assistant running tool | **Shows** | - | Running |
| Assistant finished asking | **Hides** | - | Running + 3s grace |
| Backend idle | **Hides** | - | Stopped |

---

## Merge History

### Merge 1 (d1bdab10)
- Merged 8 commits from dev
- Conflicts: settings-page.tsx, settings-route.tsx
- Session files: no conflicts

### Merge 2 (8029ce6)
- Merged 13 new commits from dev
- No conflicts
- Preserved all domework fixes

---

## Files Modified

- `apps/app/src/react-app/domains/session/surface/session-surface.tsx`
- `apps/app/src/react-app/domains/session/surface/message-list.tsx`
- `apps/app/src/react-app/domains/session/surface/composer/composer.tsx`
- `skills/tim-tvpool-1/skill.md`
