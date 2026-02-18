# .NET Core Migration Guide

When migrating this solution to .NET Core, follow these steps:

## 1. Create Runtime Config File

Create a file `CppCliInterop/CppCliInterop.runtimeconfig.json` with the following content:

```json
{
  "runtimeOptions": {
    "tfm": "netcoreapp3.1",
    "framework": {
      "name": "Microsoft.WindowsDesktop.App",
      "version": "3.1.0"
    }
  }
}
```

**Note:** This file is only needed if using Visual Studio 2019 versions earlier than 16.5 preview 2. Later versions generate this automatically.

## 2. Update CppCliInterop Project File

In `CppCliInterop/CppCliInterop.vcxproj`:

1. Change `<CLRSupport>true</CLRSupport>` to `<CLRSupport>NetCore</CLRSupport>` (this tells the compiler to use `/clr:netcore` instead of `/clr`)
   - Note: This setting appears in multiple configuration/platform-specific property groups, so update all occurrences

2. Replace `<TargetFrameworkVersion>v4.7</TargetFrameworkVersion>` with `<TargetFramework>netcoreapp3.1</TargetFramework>`

3. Replace the .NET Framework references:
   ```xml
   <Reference Include="System" />
   <Reference Include="System.Data" />
   <Reference Include="System.Windows.Forms" />
   <Reference Include="System.Xml" />
   ```
   
   With:
   ```xml
   <FrameworkReference Include="Microsoft.WindowsDesktop.App.WindowsForms" />
   ```

## 3. Update ManagedLibrary Project

Migrate the `ManagedLibrary` C# project from .NET Framework 4.7 to .NET Core 3.1 or later.
