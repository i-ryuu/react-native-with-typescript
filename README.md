# ReactNative with TypeScript project sample

ReactNativeをTypeScriptを使ってtscでコンパイルできるところまでのメモ。

## 環境
- node 10.4.1
- macOS 10.13.5
- react-native-cli 2.0.1
- TypeScript 2.9.1

## エラーと解決策

[Facebook公式のブログ](https://facebook.github.io/react-native/blog/2018/05/07/using-typescript-with-react-native.html)でもやり方が説明されているが、それをそのまま用意しても以下のようなエラー(TypeScript2.8以降の表示されるエラーっぽい。)にあたる。


```js
node_modules/@types/react-native/globals.d.ts:92:14 - error TS2300: Duplicate identifier 'RequestInfo'.

92 declare type RequestInfo = Request | string;
                ~~~~~~~~~~~


node_modules/@types/react-native/index.d.ts:3618:49 - error TS2304: Cannot find name 'Map'.

3618     static queryCache?(urls: string[]): Promise<Map<string, "memory" | "disk">>;
                                                     ~~~


node_modules/@types/react-native/index.d.ts:3638:42 - error TS2304: Cannot find name 'Map'.

3638     queryCache?(urls: string[]): Promise<Map<string, "memory" | "disk">>;
                                              ~~~


node_modules/@types/react-native/index.d.ts:8870:11 - error TS2451: Cannot redeclare block-scoped variable 'console'.

8870     const console: Console;
               ~~~~~~~


node_modules/@types/react-native/index.d.ts:8878:18 - error TS2717: Subsequent property declarations must have the same type.  Property 'geolocation' must be of
 type 'Geolocation', but here has type 'GeolocationStatic'.

8878   e  n  readonly geolocation: Geolocation;
                      ~~~~~~~~~~~


node_modules/@types/react-native/index.d.ts:8881:11 - error TS2451: Cannot redeclare block-scoped variable 'navigator'.

8881     const navigator: Navigator;
               ~~~~~~~~~
       e  n      .

node_modules/typescript/lib/lib.d.ts:19919:13 - error TS2451: Cannot redeclare block-scoped variable 'navigator'.

19919 declare var navigator: Navigator;
                  ~~~~~~~~~


node_modules/typescript/lib/lib.d.ts:20095:13 - error TS2451: Cannot redeclare block-scoped variable 'console'.

20095 declare var console: Console;
                  ~~~~~~~


node_modules/typescript/lib/lib.d.ts:20152:6 - error TS2300: Duplicate identifier 'RequestInfo'.

20152 type RequestInfo = Request | string;
           ~~~~~~~~~~~
```


解決方法はこのissueに見つけた。
[https://github.com/DefinitelyTyped/DefinitelyTyped/issues/24573](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/24573)

TypeScriptを2.7系にダウングレードするか、tsconfig.jsonのcompilerOptions内で
```
skipLibCheck: true
```
にする必要がある。


またTypeScript化するにあたって、index.jsはReactNative上プロジェクトルートになければいけないため、かつ、そんなに処理することがないためindex.jsはTypeScriptのコンパイル対象外にして、srcsディレクトリと対象とする。ts(tsx)のコンパイル後のファイルはbuildディレクトリに出力する。

