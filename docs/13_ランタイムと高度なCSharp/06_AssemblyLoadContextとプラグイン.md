# AssemblyLoadContext とプラグイン

## 目的

`AssemblyLoadContext` を使った assembly load の分離と plugin model の入口を理解します。

## 前提

- [アセンブリとメタデータ](05_アセンブリとメタデータ.md) を読んでいる

## 要点

- `AssemblyLoadContext` は assembly を読み込む context です。
- plugin ごとに依存関係を分離したい場合に使います。
- unload 可能な context を作ると、plugin を破棄できる設計にできます。
- 実装は難しいため、まず公式 sample と既存 library を確認します。

## コード例

```csharp
// この例では「AssemblyLoadContext とプラグイン」の要点を最小のコードで確認します。
using System.Reflection;
using System.Runtime.Loader;

var path = Path.GetFullPath("Plugin.dll");
var loadContext = new AssemblyLoadContext("Plugin", isCollectible: true);

Assembly assembly = loadContext.LoadFromAssemblyPath(path);
Console.WriteLine(assembly.FullName);

loadContext.Unload();
```

## plugin contract の例

```csharp
public interface IPluginCommand
{
    string Name { get; }

    Task ExecuteAsync(CancellationToken cancellationToken);
}
```

host application と plugin が同じ contract assembly を参照すると、host は plugin の具象型を知らなくても interface 経由で呼び出せます。

## plugin discovery の例

```csharp
var pluginTypes = assembly.GetTypes()
    .Where(type => typeof(IPluginCommand).IsAssignableFrom(type))
    .Where(type => type is { IsAbstract: false, IsInterface: false });

foreach (var pluginType in pluginTypes)
{
    if (Activator.CreateInstance(pluginType) is IPluginCommand command)
    {
        await command.ExecuteAsync(cancellationToken);
    }
}
```

## コードの読み方

`AssemblyLoadContext` を作り、指定 path の assembly を読み込んでいます。`isCollectible: true` にすると unload 可能な context になります。ただし、読み込んだ型や instance への参照が残っていると unload されません。

`IPluginCommand` は host と plugin の境界です。`assembly.GetTypes()` で plugin assembly 内の型を探し、interface を実装している具象型だけを作成しています。実務では DI、constructor parameter、例外処理、timeout、権限確認を追加します。

## 実務での使い方

plugin architecture、script runner、拡張機能、tooling で使います。通常の Web API や業務アプリでは必要になることは少なく、依存関係を複雑にしやすいため慎重に使います。

plugin を unload したい場合は、plugin instance、Type、Assembly、delegate、event handler への参照を残さないようにします。load error が起きたら、plugin 本体だけでなく依存 assembly の version と配置場所も確認します。

## よくあるミス

- plugin assembly への参照を残し、unload できない。
- default context と plugin context の依存関係を混ぜる。
- version 違いの依存 assembly を想定せず load error にする。
- plugin から host の内部型へ直接依存させ、contract 境界を壊す。

## 練習問題

1. `AssemblyLoadContext.Default` の意味を調べる。
2. unload 可能な context が回収されない原因を調べる。
3. plugin を interface 経由で呼ぶ設計を考える。
4. plugin の依存 assembly が見つからないときの調査手順を説明する。

## 関連リンク

- [AssemblyLoadContext Class](https://learn.microsoft.com/dotnet/api/system.runtime.loader.assemblyloadcontext)
- [Create a .NET Core application with plugins](https://learn.microsoft.com/dotnet/core/tutorials/creating-app-with-plugin-support)
