# Google Apps Script (GAS) TypeScript プロジェクト開発ルール

このプロジェクトは、TypeScript で記述されたソースコードを `esbuild` でビルドし、`clasp` を使用して Google Apps Script にデプロイし、**Web アプリ（Web App）として起動・実行する**ものです。

## 1. プロジェクト構成と開発フロー
- **ソースコードの変更**: 必ず `src/` ディレクトリ内の TypeScript ファイルを編集してください。
- **ビルド出力**: `dist/` ディレクトリはビルド成果物（`main.js` や `appsscript.json`）が出力される場所です。**`dist/` 内のファイルは直接編集しないでください。**
- **デプロイ**: コードを変更した後は、ビルドを行ってから `clasp push` を行います。
  - ビルドコマンド: `npm run build` または `node esbuild.js`
- **ドキュメントの更新**: 実装内容の追加や変更を行った場合は、それに合わせてプロジェクトの [README.md](file:///Users/xabibmf/program/gas/gas-ts-starter/README.md) も必ず修正・更新してください。

## 2. エントリーポイント（HTTPメソッド）の設計
本プロジェクトは Web アプリとして動作するため、プログラムは HTTP メソッドから開始されるように実装します。
- **開始関数**: `doGet` または `doPost` 関数を用意し、これらの関数からメインの処理が開始されるように設計してください。
- **公開用ファイル**: [src/main.ts](file:///Users/xabibmf/program/gas/gas-ts-starter/src/main.ts)
  - `doGet` や `doPost` などのエントリーポイント関数は、GAS 側から Web アプリのエンドポイントとして認識されるよう、必ず `src/main.ts` を通じてグローバルスコープに露出（公開）させてください。

## 3. GAS 上でのデバッグ用関数（`debugRun`）の作成
パラメータ（引数）を受け取る `doGet` や `doPost` などの処理を実装する場合、GAS のオンラインエディタ上で動作確認やデバッグ実行ができるように、擬似的なパラメータを渡して実行するデバッグ用の関数を作成してください。
- **関数名**: `debugRun` (または `debugRunDoPost` など、役割が明確な名前)
- **実装内容**: テスト用のイベントオブジェクト（`e`）を組み立てて、`doPost(e)` や `doGet(e)` を呼び出すだけの関数。これにより、GAS のエディタから直接実行してログ確認などが可能になります。

## 4. 非同期処理に関する重要な制約
Google Apps Script では、特に Web アプリのエンドポイント（`doPost` や `doGet`）は**同期関数**として実行されます。これらの関数内で非同期処理的の完了を待機することはできません。

### ❌ 避けるべき実装パターン
`async/await` や `Promise.then()` を使用して、Web アプリのレスポンス内で非同期処理を待とうとすると、正しく動作しないかタイムアウトが発生します。
```typescript
// 動作しません
export const doPost = (e: GoogleAppsScript.Events.DoPost) => {
  asyncFunction().then((res) => { ... });
};
```

### ✅ 推奨される実装パターン
- `doGet` や `doPost` は `async` キーワードを使わず、**同期関数**として実装してください。
- 外部 API との通信には、同期的に実行される `UrlFetchApp.fetch()` を直接使用してください。
```typescript
// 推奨される同期的な実装
export const doPost = (e: GoogleAppsScript.Events.DoPost) => {
  const response = UrlFetchApp.fetch(url, options);
  const data = JSON.parse(response.getContentText());
  return ContentService.createTextOutput(JSON.stringify(data));
};
```

## 5. コーディング時のチェックリスト
- [ ] 編集しているファイルは `src/` 配下の TypeScript ファイルか？
- [ ] `doPost`/`doGet` から処理が開始される構成になっているか？
- [ ] パラメータを受け取る場合、デバッグ用の `debugRun` 関数を用意したか？
- [ ] `doPost`/`doGet` などのエンドポイント関数に `async` が付いていないか？
- [ ] 実装内容に合わせて [README.md](file:///Users/xabibmf/program/gas/gas-ts-starter/README.md) を更新したか？
