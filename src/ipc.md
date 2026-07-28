---
updated: 2026-07-05
---

# IPC 机制

## 名称

- Chromium: `mojo`
- Firefox: IPDL 语言, `Electrolysis`
- WebKit: `CoreIPC` 位于 `Source/WebKit/Platform/IPC`
- Servo: `generic_channel`

## 底层原理

实际上是一样的：

- Linux/macOS: Unix domain socket
- Windows: Named Pipes
- 或者 macOS/iOS: mach porets

## WebKit 实现细节

位于 `Source/WebKit/Platform/IPC/*.c`.

```ts
// Source/WebKit/Platform/IPC/Connection.cpp
class Connection {
  m_dispatcher: Weak<SerialFunctionDispatcher>,
  
  send<T>(message: T);
  sendSync<T>();
}

m_dispatcher.deref().dispatch()
```

继承 MessageReceiver.
