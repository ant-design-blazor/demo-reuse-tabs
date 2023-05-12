# ReuseTabs

A reuse tabs demo for [Ant Design Blazor](https://github.com/ant-design-blazor/ant-design-blazor).

English | [简体中文](README-zh_CN.md)

# Demo

https://antblazor.com/demo-reuse-tabs/

# ScreenShot

![demo](./assets/reuse-tabs-demo1.gif)

# How to use

## Prerequisites

Follow the [installation steps of AntDesign](https://antblazor.com/docs/introduce) and install the AntDesign dependencies.

## Basic case

1. First of all, create a blazor project using `dotnet new` command.

2. Modify the `App.razor` file, warp the `RouteView` with `<CascadingValue Value="routeData">`.

   ```diff
   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
   +       <CascadingValue Value="routeData">
               <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" / >
   +        </CascadingValue>
      </Found>
       ...
   </Router>
   ```

3. Then modify the `MainLayout.razor` file, add the `ReuseTabs` component. Note that `@Body` is not required at this case.

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
   +       <ReuseTabs Class="top-row px-4" TabPaneClass="content px-4" />
       </div>
   </div>

   ```

## Customize tab title

- If it's just text, you can use the `ReuseTabsPageTitle` attribute.

  ```diff
  @page "/counter"
  + @attribute [ReuseTabsPageTitle("Counter")]
  ```

- If you want to use a template or variables, then implement the `IReuseTabsPage` interface and implement the method

  ```diff
  @page "/order/{OrderNo:int}"
  + @implements IReuseTabsPage

  <h1>Hello, world!</h1>

  @code{
      
      [Parameter]
      public int OrderNo { get; set; }

  +   public RenderFragment GetPageTitle() =>
  +       @<div>
  +           <Icon Type="home"/> Order No. @OrderNo
  +       </div>
  +   ;
  }
  ```

## Authentication

ReuseTabs can be integrated with Blazor's Authentication component.

1. Start by adding the authentication component according to the official documentation, [_ASP.NET Core Blazor authentication and authorization_](https://docs.microsoft.com/en-us/aspnet/core/blazor/security/?view=aspnetcore-6.0&WT.mc_id=DT-MVP-5003987).

2. Install our `AntDesign.Components.Authentication` Nuget package.

    ```bash
    $ dotnet add package AntDesign.Components.Authentication
    ```

3. Warp the `AuthorizeRouteView` with `<CascadingValue Value="routeData">`.

    ```diff
    <CascadingAuthenticationState>
        <Router AppAssembly="@typeof(Program).Assembly">
            <Found Context="routeData">
    +           <CascadingValue Value="routeData">
                    <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    +           </CascadingValue>
            </Found>
            <NotFound>
                <LayoutView Layout="@typeof(MainLayout)">
                    <p>Sorry, there's nothing at this address.</p>
                </LayoutView>
            </NotFound>
        </Router>
    </CascadingAuthenticationState>
    ```

The rest of the configuration is the same as the official documentation and ReuseTabs.

## More configurations

You can set more options by using `ReuseTabsPage` attribute above pages.

```diff
@page "/counter"
+ @attribute [ReuseTabsPage(Title = "Home", Closable = false)]
```

If you want to ignore any pages, you can setting the `Ignore=true` in `ReuseTabsPage` attribute.

```diff
@page "/counter"
+ @attribute [ReuseTabsPage(Ignore = true)]
```

See the API for more configurations.

## API

### ReuseTabsPageAttribute

| Property | Description | Type | Default | 
| --- | --- | --- | --- |
| Title | The fixed title show on tab. | string | current path |
| Ignore | If `Ignore=true`, the page is not displayed in tab, but in the entire page. | boolean | false |
| Closable | Whether the delete button can be displayed. | boolean | false |
| Pin | Whether the page is fixed to load and avoid closing, usually used on the home page or default page. | boolean | false |

### ReuseTabsService

| Method | Description | Type | Default | 
| --- | --- | --- | --- |
| ClosePage(string key) | Close the page with the specified key. | string | current path |
| CloseOther(string key) | Close all pages except those that specify key, `Cloasable=false`, or `Pin=true`. | boolean | false |
| CloseAll() | Close all pages except those that `Cloasable=false` or `Pin=true`. | boolean | false |
| CloseCurrent() | Close current page. | boolean | false |
| Update() | Update the state of current tab. When the variable referenced in `GetPageTitle()` changes, `Update()` needs to be called to update the tab display. | boolean | false |