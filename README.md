# VSCode での Typescript デバッグ入門

## 環境準備

### モジュールインストール

```bash
npm init -y
npm install --save-dev typescript ts-node
```

### Typescript 用設定ファイル

以下のコマンドでファイルを作成する

```bash
npx tsc --init
```

作成された tsconfig.json の中身を以下のようにする。
ほぼデフォルトのままで、sourceMap の項目だけ変更している。

```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "esModuleInterop": true,
    "sourceMap": true, //これの記述を追加
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

## デバッグ対象のコード準備

```ts
function add(a: number, b: number): number {
  return a + b;
}

const result = add(1, 2);
console.log(`result is ${result}`);
```

## デバッグ用の設定ファイル準備

launch.json を作成して、以下のように記述する。
VSCode の作成機能で作成したものに、runtimeArgs と resoleveSourceMapLocations の項目を追加した。

```json
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "プログラムの起動",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}/index.ts",
      "outFiles": ["${workspaceFolder}/**/*.js"],
      "runtimeArgs": ["-r", "ts-node/register"], // これを追加
      "resolveSourceMapLocations": [ // これを追加
        "${workspaceFolder}/**",
        "!**/node_modules/**"
      ]
    }
  ]
}
```

## デバッグ実行

通常の VSCode のデバッグを実行する。
