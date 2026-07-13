<div align="center">
  <h1>Sileo</h1>
  <p>An opinionated, physics-based toast component for React.</p>
  <p><a href="https://sileo.aaryan.design">Try Out</a> &nbsp; / &nbsp; <a href="https://sileo.aaryan.design/docs">Docs</a></p>
  <video src="https://github.com/user-attachments/assets/a292d310-9189-490a-9f9d-d0a1d09defce"></video>
</div>

## Installation

```bash
npm i sileo
```

## Getting Started

Render one `Toaster` near the root of your app, then call `push` or `sileo`
from anywhere in your client-side code.

```tsx
import { push, Toaster } from "sileo";

export default function App() {
  return (
    <>
      <Toaster position="top-right" />
      <YourApp />
    </>
  );
}
```

```tsx
push.success({ title: "Saved" });
```

For detailed docs, visit https://sileo.aaryan.design.

## Basic Toasts

```tsx
push.show({ title: "Default toast" });
push.success({ title: "Saved" });
push.error({ title: "Something went wrong" });
push.warning({ title: "Check this first" });
push.info({ title: "Heads up" });
```

Add details, custom duration, or actions when you need them.

```tsx
push.action({
  title: "New message",
  description: "Aaryan sent you a message.",
  button: {
    title: "Open chat",
    onClick: () => router.push("/chat/123"),
  },
});
```

## Async Toasts

### Automatic promise tracking

Use `push.promise` when you already have a promise and want Sileo to update the
same toast when it resolves or rejects.

```tsx
push.promise(fetchData(), {
  loading: "Loading...",
  success: "Done!",
  error: "Failed",
});
```

You can also return full toast options from success/error callbacks.

```tsx
push.promise(fetchUsers(), {
  loading: "Loading users...",
  success: (users) => ({
    title: "Users loaded",
    description: `${users.length} users are ready.`,
  }),
  error: (error) => ({
    title: "Could not load users",
    description: error instanceof Error ? error.message : "Please try again.",
  }),
});
```

### Manual resolve/reject flow

Use the manual controller when a `try`/`catch` flow is clearer. This matches the
simple style you asked for: create the loading toast first, then resolve or
reject it later.

```tsx
const notification = push.promise("We're sending your message, hold on...");

try {
  const { chatId } = await sendMsg(user.id, message);

  notification.resolve({
    message: `Your message has been successfully sent to ${user.name}.`,
    props: {
      chatUrl: `/chat/${chatId}`,
    },
  });
} catch {
  notification.reject("Message failed to send.");
}
```

The controller also exposes `notification.update(...)` for intermediate states
and `notification.dismiss()` when you want to close it yourself.

## Updating Toasts

Use `push.loading` plus `push.update` when you want to keep full control with a
stable toast id.

```tsx
const id = push.loading({ title: "Uploading" });

await uploadFile();

push.update(id, {
  title: "Uploaded",
  description: "Your file is ready.",
  state: "success",
});
```

## Options

Most APIs accept the same toast options.

```ts
type SileoOptions = {
  id?: string;
  state?: "success" | "loading" | "error" | "warning" | "info" | "action";
  title?: string;
  description?: React.ReactNode | string;
  position?:
    | "top-left"
    | "top-center"
    | "top-right"
    | "bottom-left"
    | "bottom-center"
    | "bottom-right";
  duration?: number | null;
  icon?: React.ReactNode | null;
  fill?: string;
  roundness?: number;
  autopilot?: boolean | { expand?: number; collapse?: number };
  button?: {
    title: string;
    onClick: () => void;
  };
};
```

Set `duration: null` for a persistent toast. Omit `id` for a new toast, or pass a
stable `id` when you intentionally want future calls to update the same toast.
