# xcx-g2s-meter

[AkaDako](https://akadako.com/) ボード（S-LINK）を使って **電圧・電流・電力** を計測するための [Xcratch](https://xcratch.github.io/) 拡張機能です。

[tfabworks/xcx-g2s](https://github.com/tfabworks/xcx-g2s) をベースに、電流・電圧・電力メーター（参考: <https://699.jp/wattmeter/>）の計測ブロックを追加しています。AkaDako の既存ブロック（Grove センサー／アクチュエーター、アナログ・デジタル入出力など）もそのまま利用できます。

## ✨ 追加した計測ブロック

| ブロック | 説明 | 入力コネクタ | 範囲 |
| --- | --- | --- | --- |
| `電圧 [V]` | 計測した電圧を返す | アナログ **A1** | 0 〜 20 V |
| `電流 [A]` | 計測した電流を返す | アナログ **A2** | 0 〜 2 A |
| `電力 [W]` | 電圧 × 電流 を返す | A1・A2 から算出 | 0 〜 40 W |

いずれも値を返すレポーターブロックです。ボード未接続のときは空文字を返します。

### 計測のしくみ

参考サイト <https://699.jp/wattmeter/> と同じ換算を行っています。AkaDako のアナログ入力レベル（0〜100）に係数を掛けて実量に変換します。

- 電圧 = アナログ A1 のレベル × 0.2 （0〜100 → 0〜20 V）
- 電流 = アナログ A2 のレベル × 0.02 （0〜100 → 0〜2 A）
- 電力 = 電圧 × 電流
- レベルが 0.1 以下のときはノイズ・未接続とみなして 0 とします。

## 🔌 配線

- 電圧センサーの出力を **アナログ A1** に接続します。
- 電流センサーの出力を **アナログ A2** に接続します。

## 🚀 Xcratch での使い方

この拡張は [Xcratch](https://xcratch.github.io/) 上で他の拡張と一緒に使えます。

1. [Xcratch エディター](https://xcratch.github.io/editor) を開く
2. 「拡張機能を追加」ボタンをクリック
3. 「Extension Loader」拡張を選ぶ
4. 入力欄に次のモジュール URL を入力する

```
https://aoki-tfab.github.io/xcx-g2s-meter/dist/g2s.mjs
```

5. 「ボードを接続する」ブロックで AkaDako（S-LINK）に接続してから、電圧・電流・電力ブロックを使います。

> 接続には Web MIDI / Web Serial を使うため、Google Chrome などの対応ブラウザーで開いてください。

## 🛠 開発

### 必要環境

- Node.js（LTS 推奨）

### セットアップとビルド

```sh
npm install
npm run build
```

`npm run build` はベースの [scratch-gui](https://github.com/xcratch/scratch-gui) を `../scratch-gui` に取得してから、拡張モジュールを `dist/g2s.mjs` にバンドルします。

> ローカルの Xcratch にインストールして開発する `npm run register` は、scratch-vm 配下へのシンボリックリンク作成を伴うため、Windows では管理者権限／開発者モードが必要です。`dist/g2s.mjs` のビルドだけなら `node scripts/build.js` を直接実行しても作成できます。

## 📦 公開（GitHub Pages）

`dist/g2s.mjs` を GitHub Pages で配信し、上記のモジュール URL から読み込みます。

ホームページ: <https://aoki-tfab.github.io/xcx-g2s-meter/>

## 🙏 クレジット

- ベース拡張: [tfabworks/xcx-g2s](https://github.com/tfabworks/xcx-g2s)（MIT License）
- 計測ロジックの参考: [AkaDako 電流・電圧・電力メーター](https://699.jp/wattmeter/) / [AkaDako](https://akadako.com/)
- 拡張プラットフォーム: [Xcratch](https://xcratch.github.io/)

## 📝 ライセンス

ベースリポジトリにならい [MIT License](./LICENSE) で公開しています。
