
# WinUI 2 & 3 Repro Template

This is a template for reproducing issues across UWP and the [Windows App SDK](https://aka.ms/windowsappsdk) with [WinUI 2 & 3](https://aka.ms/winui) in a clean environment.

It allows you to write XAML and/or code once in a project and run it on both platforms easily.

Outside of sharing/linking of the core files, the projects are setup as `File->New->Project`s to act as they would under the same circumstances of any other application. The only 'magic' here is using the same application/root namespace across projects and standard csproj file reference linking to have single copies of the files copied across projects automatically.

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

## Template Information

Find out more about using the template itself in the [project's readme file](project_readme.md).

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

Licensed under CC0, see the [license file](LICENSE.TXT) for details.
