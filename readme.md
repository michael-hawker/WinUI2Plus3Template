
# WinUI 2 & 3 Repro Template

This is a template for reproducing issues across UWP and the Windows App SDK with WinUI 2 & 3 in a clean environment.

It allows you to write XAML and/or code once in a project and run it on both platforms easily.

Outside of sharing/linking of the core files, the projects are setup as `File->New->Project`s to act as they would under the same circumstances of any other application. The only 'magic' here is using the same application/root namespace across projects and standard csproj file reference linking to have single copies of the files copied across projects automatically.

## Current Template Versions

- WinUI 2.8.3
- WindowsAppSDK 1.2.230118.102

## Using this template

To use this template to start a new project, clone this repro and in the repo root run:

```cmd
dotnet new install .\
```

Then navigate to where you'd like to create your new project and run:

```cmd
dotnet new winui2plus3 -n YourProjectNameHere
```

This will create a new folder called `YourProjectNameHere` which contains the contents of this template with the new name. Open up the solution file there and modify the `MainPage.xaml/cs` to create your scenario.

Then you can run either `YourProjectNameHere.Uwp` or `YourProjectNameHere.WinAppSDK` to run the code on either platform to compare behavior.

## Linked Files

The following files are linked by default:

- App.xaml _(sets up the WinUI2/3 resources)_
- MainPage.xaml _(main place to write **your** code)_
- MainPage.xaml.cs _(main place to write **your** code)_
- Llama.jpg _(in case you need an image asset_ 🦙 _)_
- `Assets` Folder _(to reduce disk space)_

## Adding new files

To add a new file to a project, add it to the **WinAppSDK** project first, then add it to the _UWP_ project but as an `Add->Existing Item` and `Add As Link`.

Be sure to remove the default namespaces on XAML pages and replace with the `#if` conditional from the initial example page.

(_Global namespaces could make this simpler, but want to make this explicit in case other namespaces are required for bug reproduction. i.e. hides less things and less 'magic'._)

## Adding Dependencies

If you need to add extra dependencies, you need to add them to both project heads independently. If it's a **.NET Standard 2.0** project _(required for UWP)_, then just add it to both projects as normal and use as normally.

If it's a dependency which has different packages per-version, be sure not only install the correct UWP vs. WindowsAppSDK based version of the package in each project head, but also include any namespaces in your `#if` at the top of your file when referencing code using it.

## Adding XAML Component Dependencies

Hopefully, your XAML component shares the **same namespace** for its UWP and WindowsAppSDK versions across their packages. _If not_, we recommend you request this for the next release from your component vendor to support easier migration between UWP and the WindowsAppSDK in the future; as otherwise it makes sharing XAML code between both frameworks difficult (if not impossible).

_If the namespace is the same_, you should be able to follow the regular dependency instructions above without issue; just add the `xmlns` definition to the top of your XAML file with the shared namespace.

If the namespace is **not** the same, then check if the control you wish to use is `sealed`. If it is **sealed**, there's not much you can do outside of copying the source of the component, if available, over to this project or duplicating your repro XAML files across the heads (mostly defeating this template's purpose). 😟

If it is **not** `sealed`, you can perform the following trick by creating a new C# file in the WindowsAppSDK project (and then link to the UWP project):

```cs
namespace ProjectTemplate; // Replace with your namespace/project name

#if WINDOWS_UWP
using MyComponent = Whatever.UWP.ComponentName;
#else
using MyComponent = Whatever.WinUI.ComponentName;
#endif

public partial class MyNewComponent : MyComponent
{
}
```

And that's it! This code will basically create a common namespace for you to use the XAML component in both WinUI 2 on UWP and WinUI 3 in the WindowsAppSDK. Just use from the namespace you provided in your XAML, e.g.:

```xml
<Page xmlns:local="using:ProjectTemplate">
  <local:MyNewComponent/>
```

## How this template was created

This template was created using the `dotnet` [templating tools](https://learn.microsoft.com/dotnet/core/tools/custom-templates) and mapping parallel names/structures across a `File->New->Project` for a UWP and Windows App SDK app. Mostly around using the same root namespace and then linking files _(Add->Existing Item, Add As Link)_.

1. `File->New->Project` was used for both apps in Visual Studio:

    - Blank App, Packaged (WinUI 3 in Desktop)
    - Blank app (Universal Windows)

2. The `Assets` folder images were deleted from the UWP head, and linked to their exact same counterpart images in the Windows App SDK folder (to save disk/repo space).

3. `<LangVersion>11.0</LangVersion>` was added to the UWP project to allow for using newer C# language features not dependant on the base runtime; like file-scoped namespaces, this was applied to `App.xaml.cs` for instance to reduce indentation.

4. The root/assembly namespace for both projects was flattened to just be `ProjectTemplate` to allow easier code sharing between the two applications.

5. Installed WinUI 2 (`Microsoft.UI.Xaml`) in the UWP head and updated to the latest Windows App SDK stable version.

6. `App.xaml` was removed in the UWP project and linked to the one in the Windows App SDK project. **Note:** `App.xaml.cs` is independent for the two applications as the launch process is different, and we didn't want to substantially change the default template setup. If you need to setup custom launch behavior, we suggest creating a common helper class and then calling that from both places in the two different `App.xaml.cs` files.

7. A `MainPage.xaml` was added to the WindowsAppSDK head and referenced directly by `MainWindow.xaml` (as Window isn't a `FrameworkElement`).

8. The UWP head had its `MainPage.xaml` removed and linked to the one in the WindowsAppSDK project. (As was the backing `MainPage.xaml.cs` code file.)

9. The using directives were added to the top of `MainPage.xaml` to differentiate between the two namespaces for `Windows.UI.Xaml` and `Microsoft.UI.Xaml`.

## Code of Conduct

This repo abides by the [Microsoft Open Source Code of Conduct](CODE_OF_CONDUCT.md).

## Trademark Notice

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow Microsoft’s Trademark & Brand Guidelines. Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship. Any use of third-party trademarks or logos are subject to those third-party’s policies.

## License

Licensed under CC0, see [license.txt](license.txt) for details.
