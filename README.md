<div align="center">
  <h1>Sileo</h1>
  <p>An opinionated, physics-based toast component for React.</p>
  <p><a href="https://sileo.aaryan.design">Try Out</a> &nbsp; / &nbsp; <a href="https://sileo.aaryan.design/docs">Docs</a></p>
  <video src="https://github.com/user-attachments/assets/a292d310-9189-490a-9f9d-d0a1d09defce"></video>
</div>

### Installation

```bash
npm i sileo
```

### Getting Started

```tsx
import { push, sileo, Toaster } from "sileo";

export default function App() {
  return (
    <>
      <Toaster position="top-right" />
      <YourApp />
    </>
  );
}
```

For detailed docs, click here: https://sileo.aaryan.design


### Async Toasts

For async work, use simple strings when you only need a title. Sileo keeps the
loading toast open, then updates the same toast when the promise resolves or
rejects.

```tsx
sileo.promise(fetchData(), {
  loading: "Loading...",
  success: "Done!",
  error: "Failed",
});
```


You can also manually resolve or reject a loading toast when a `try`/`catch`
flow reads better than passing the promise upfront.

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

Use objects only when you need extra details, such as a description or button.

```tsx
sileo.promise(fetchData(), {
  loading: "Loading...",
  success: (data) => ({
    title: "Done!",
    description: `${data.length} items loaded.`,
  }),
  error: { title: "Failed", description: "Please try again." },
});
```

### Updating Toasts

Use a stable `id` when you want to replace a toast in-place, or call
`sileo.update(id, options)` when a background task changes state.

```tsx
const id = sileo.loading({ title: "Uploading" });

await uploadFile();

sileo.update(id, {
  title: "Uploaded",
  description: "Your file is ready.",
  state: "success",
});
```
