<!-- GENERATED FILE, DO NOT EDIT -->

# API

API reference for [`@webext-core/messaging`](/guide/messaging/).

:::info
The entire API reference is also available in your editor via [JSDocs](https://jsdoc.app/).
:::

## `defineExtensionMessaging`

```ts
function defineExtensionMessaging<
  TProtocolMap = Record<string, ProtocolWithReturn<any, any>>
>(config?: ExtensionMessagingConfig): Messenger<TProtocolMap> {
  // ...
}
```

Creates a new `Messenger` used to send and recieve messages in your extension.

### Parameters

- ***`config?: ExtensionMessagingConfig`***<br/>Configure the behavior of the messenger.

## `ExtensionMessagingConfig`

```ts
interface ExtensionMessagingConfig {
  logger?: Logger | null;
}
```

Cofnigure how the messenger behaves.

### Properties 

- ***`logger?: Logger | null`*** (default: `console`)<br/>The logger to use when logging messages. Set to `null` to disable logging.

## `GetDataType`

```ts
type GetDataType<T> = T extends (...args: infer Args) => any
  ? Args["length"] extends 0 | 1
    ? Args[0]
    : never
  : T extends ProtocolWithReturn<any, any>
  ? T["BtVgCTPYZu"]
  : T;
```

Given a function declaration, `ProtocolWithReturn`, or a value, return the message's data type.

## `GetReturnType`

```ts
type GetReturnType<T> = T extends (...args: any[]) => infer R
  ? R
  : T extends ProtocolWithReturn<any, any>
  ? T["RrhVseLgZW"]
  : void;
```

Given a function declaration, `ProtocolWithReturn`, or a value, return the message's return type.

## `Logger`

```ts
interface Logger {
  debug(...args: any[]): void;
  log(...args: any[]): void;
  warn(...args: any[]): void;
  error(...args: any[]): void;
}
```

Interface used to log text to the console when sending and recieving messages.

## `MaybePromise`

```ts
type MaybePromise<T> = Promise<T> | T;
```

Either a Promise of a type, or that type directly. Used to indicate that a method can by sync or
async.

## `Messenger`

```ts
interface Messenger<TProtocolMap> {
  sendMessage<TType extends keyof TProtocolMap>(
    type: TType,
    data: GetDataType<TProtocolMap[TType]>,
    tabId?: number
  ): Promise<GetReturnType<TProtocolMap[TType]>>;
  onMessage<TType extends keyof TProtocolMap>(
    type: TType,
    onReceived: OnMessageReceived<TProtocolMap, TType>
  ): RemoveListenerCallback;
  removeAllMessageListeners(): void;
}
```

Use the functions defined in the messenger to send and recieve messages throughout your entire extension.

Unlike the regular `chrome.runtime` messaging APIs, there are no limitations to when you call `onMessage` or `sendMessage`.

## `ProtocolWithReturn`

:::danger Deprecated
Use the function syntax instead: <https://webext-core.aklinker1.io/messaging/protocol-maps.html#syntax>
:::

```ts
interface ProtocolWithReturn<TData, TReturn> {
  BtVgCTPYZu: TData;
  RrhVseLgZW: TReturn;
}
```

Used to add a return type to a message in the protocol map.

> Internally, this is just an object with random keys for the data and return types.

### Properties 

- ***`BtVgCTPYZu: TData`***<br/>Stores the data type. Randomly named so that it isn't accidentally implemented.

- ***`RrhVseLgZW: TReturn`***<br/>Stores the return type. Randomly named so that it isn't accidentally implemented.

### Examples

```ts
interface ProtocolMap {
  // data is a string, returns undefined
  type1: string;
  // data is a string, returns a number
  type2: ProtocolWithReturn<string, number>;
}
```

## `RemoveListenerCallback`

```ts
type RemoveListenerCallback = () => void;
```

Call to ensure an active listener has been removed.

If the listener has already been removed with `Messenger.removeAllListeners`, this is a noop.

<br/><br/>

---

_API reference generated by [`plugins/typescript-docs.ts`](https://github.com/aklinker1/webext-core/blob/main/docs/.vitepress/plugins/typescript-docs.ts)_