# ReuseTabs

[Ant Design Blazor](https://github.com/ant-design-blazor/ant-design-blazor) 多标签页组件。

[English](README.md) | 简体中文

# 实例

https://antblazor.com/demo-reuse-tabs/

# 截图

![demo](./assets/reuse-tabs-demo1.gif)

# 如何使用

## 前置条件

先按照 Ant Design 的文档安装 AntDesign 组件 Nuget 包。

## 基础使用

1. 首先，使用 `dotnet new` 命令创建一个 Blazor 项目。

2. 修改项目中的 `App.razor` 文件，使用 `ReuseTabsRouteView` 替换 `RouteView`。

   ```diff
   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
   -       <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" / >
   +       <ReuseTabsRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
       </Found>
       ...
   </Router>

   ```

3. 修改 `MainLayout.razor` 文件, 增加 `ReuseTabs` 组件。注意 `@Body` 是不需要的。

   ```diff
   @inherits LayoutComponentBase

   <div class="page">
       <div class="sidebar">
           <NavMenu />
       </div>

       <div class="main">
   -       <div class="top-row px-4">
   -           <a href="http://blazor.net" target="_blank" class="ml-md-auto">About</a>
   -       </div>
   -       <div class="content px-4">
   -           @Body
   -       </div>
   +       <ReuseTabs Class="top-row px-4" TabPaneClass="content px-4" / >
       </div>
   </div>

   ```

## 自定义标签标题

- 如果只是文本标题，只需在页面上增加 `ReuseTabsPageTitle` 特性。

  ```diff
  @page "/counter"
  + @attribute [ReuseTabsPageTitle("Counter")]
  ```

- 如果需要使用模板来设置复杂的标题, 则需要页面组件实现 `IReuseTabsPage` 接口和 `GetPageTitle` 方法，就返回动态的标题了。

  ```diff
  @page "/"
  + @implements IReuseTabsPage

  <h1>Hello, world!</h1>

  @code{
  +   public RenderFragment GetPageTitle() =>
  +       @<div>
  +           <Icon Type="home"/> Home
  +       </div>
  +   ;
  }
  ```

## 更多配置

你可以通过使用 `ReuseTabsPage` 特性来设置更多选项。

```diff
@page "/counter"
+ @attribute [ReuseTabsPage(Title = "Home", Closable = false)]
```

如果你想忽略一些页面, 可以在 `ReuseTabsPage` 设置 `Ignore=true`。

```diff
@page "/counter"
+ @attribute [ReuseTabsPage(Ignore = true)]
```
