# GAS TypeScript Starter Template

Google Apps Script (GAS) を TypeScript で開発するためのスターターテンプレートです。

## 概要

このテンプレートは、TypeScript で書いたコードを Google Apps Script 向けにビルドし、clasp を使ってデプロイできる開発環境を提供します。

## 特徴

- **TypeScript**: 型安全な開発
- **esbuild**: 高速なバンドルとミニファイ
- **clasp**: Google Apps Script へのデプロイ
- **esbuild-gas-plugin**: GAS 向けに最適化されたビルド

## ファイル構成

```
gas-ts/
├── src/                  # ソースコード（TypeScript）
│   ├── App.ts           # メインのアプリケーションロジック
│   └── main.ts          # エントリーポイント（グローバルスコープへの公開）
├── dist/                 # ビルド出力先
│   ├── main.js          # ビルドされた JavaScript
│   └── appsscript.json  # GAS の設定ファイル
├── esbuild.js           # esbuild の設定
├── tsconfig.json        # TypeScript の設定
├── .clasp.json          # clasp の設定
└── package.json         # 依存関係とスクリプト
```

## セットアップ

### 1. 依存関係のインストール

```bash
npm install
```

### 2. Google Apps Script プロジェクトの作成と clasp の設定

1. [Google Apps Script](https://script.google.com/) で新しいプロジェクトを作成
2. プロジェクトのスクリプト ID を取得
3. `.clasp.json` の `scriptId` を更新

```json
{
  "scriptId": "YOUR_SCRIPT_ID_HERE",
  ...
}
```

### 3. clasp のログイン（初回のみ）

```bash
clasp login
```

## 使い方

### ビルド

TypeScript コードをビルドして `dist/` に出力します。

```bash
npm run build
```

### デプロイ

ビルドして Google Apps Script にプッシュします。

```bash
npm run deploy
```

または、個別に実行：

```bash
npm run build  # ビルド
npm run push   # プッシュ
```

## 開発の流れ

1. `src/App.ts` に処理を記述
2. 必要に応じて `src/main.ts` でグローバルスコープに公開
3. `npm run deploy` でデプロイ
4. Google Apps Script エディタまたは Web から実行

## 技術スタック

- **TypeScript**: 5.9.3
- **esbuild**: 0.25.11
- **esbuild-gas-plugin**: 0.10.0
- **@types/google-apps-script**: 2.0.7
- **clasp**: コマンドラインツール

## 注意事項

- `.clasp.json` にはスクリプト ID が含まれるため、コミット前に機密情報が含まれていないか確認してください
- `package-lock.json` は `.gitignore` に含まれています

## ライセンス

ISC






