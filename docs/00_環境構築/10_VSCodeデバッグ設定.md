# VS Code デバッグ設定

## 目的

VS Code で C# project をデバッグ実行し、breakpoint、変数確認、設定ファイルの役割を理解します。

## 前提

- [VS Code と C# Dev Kit](02_vscode-csharp.md) を完了している
- [プロジェクト作成と実行](03_プロジェクト作成と実行.md) を読んでいる
- C# project folder を VS Code で開いている

## 要点

- デバッグは、実行中の code を途中で止めて変数や呼び出し順を確認する作業です。
- C# Dev Kit を入れて project を正しく開くと、Run and Debug から C# project を実行できます。
- breakpoint は処理を止めたい行に置きます。停止したら Variables、Call Stack、Watch、Debug Console を確認します。
- `.vscode/launch.json` は「どの program をどう起動するか」、`.vscode/tasks.json` は「実行前に build するなどの task」を定義します。
- C# Dev Kit では設定を自動認識できる場合があります。設定ファイルがないこと自体は異常ではありません。
- デバッグできない場合は、project folder を開いているか、build が成功するか、C# Dev Kit が有効か、target framework が SDK と合っているかを確認します。

## breakpoint を置いて実行する

1. VS Code で C# project folder を開く。
2. `Program.cs` を開く。
3. 止めたい行の左側をクリックして breakpoint を置く。
4. `F5` または Run and Debug から実行する。
5. 停止したら Variables で値を確認する。

## 最小コードで確認する

```csharp
var unitPrice = 1200m;
var quantity = 3;
var subtotal = unitPrice * quantity;

Console.WriteLine($"小計: {subtotal}円");
```

`var subtotal = unitPrice * quantity;` の行に breakpoint を置くと、直前の `unitPrice` と `quantity` の値を確認できます。

## launch.json の例

必要になった場合は `.vscode/launch.json` で起動設定を明示できます。

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug console app",
      "type": "coreclr",
      "request": "launch",
      "program": "${workspaceFolder}/bin/Debug/net10.0/SampleApp.Console.dll",
      "cwd": "${workspaceFolder}",
      "console": "integratedTerminal",
      "preLaunchTask": "build"
    }
  ]
}
```

`program` の path は project 名や target framework に合わせて変わります。

## tasks.json の例

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build",
      "command": "dotnet",
      "type": "process",
      "args": ["build"],
      "problemMatcher": "$msCompile"
    }
  ]
}
```

## コードの読み方

デバッグ対象の code では、`unitPrice` と `quantity` が入力、`subtotal` が計算結果です。breakpoint で止めると、計算前後の値を確認できます。`launch.json` の `program` は実行する DLL、`cwd` は実行時のカレントディレクトリ、`preLaunchTask` は実行前に行う build を表します。

## 実務での使い方

実務では、エラー原因を推測だけで直すより、breakpoint で実際の値を確認する方が速いことがあります。特に入力値、条件分岐、例外が起きる直前、外部 API の response、DB から取得した値を確認するときに使います。

## よくあるミス

- `.cs` 単体ファイルだけを開き、project として認識されていない。
- build が失敗しているのに debug 設定だけを直そうとする。
- `launch.json` の `program` が古い target framework を指している。
- breakpoint が薄い表示のままで、実際には有効になっていない。
- Release build や最適化の影響で、期待通りに停止しないことを見落とす。

## 練習問題

1. `Program.cs` に breakpoint を置いて `F5` で止める。
2. Variables view で変数の値を確認する。
3. Step Over で1行ずつ進める。
4. `dotnet build` が失敗する状態にして、debug 前に build error が出ることを確認する。

## 関連リンク

- [Debug C# in Visual Studio Code](https://code.visualstudio.com/docs/csharp/debugging)
- [Visual Studio Code debugging](https://code.visualstudio.com/docs/editor/debugging)
- [Tasks in Visual Studio Code](https://code.visualstudio.com/docs/editor/tasks)
