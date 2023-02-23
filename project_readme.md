# ProjectTemplate Readme

This is a template for reproducing issues across UWP and the [Windows App SDK](https://aka.ms/windowsappsdk) with [WinUI 2 & 3](https://aka.ms/winui) in a clean environment.

It allows you to write XAML and/or code once in a project and run it on both platforms easily.

Outside of sharing/linking of the core files, the projects are setup as `File->New->Project`s to act as they would under the same circumstances of any other application. The only 'magic' here is using the same application/root namespace across projects and standard csproj file reference linking to have single copies of the files copied across projects automatically.

TODO: More information about using this template can be found below. Feel free to delete this top section's text or replace after setting up your project. For instance, add details about the bug your trying to reproduce and use this project for here. And a link to the corresponding issue: https://github.com/

## Current Template Versions

| Package Name                           | Version       |
|----------------------------------------|---------------|
| Microsoft.UI.Xaml (a.k.a. WinUI 2)     | v2.8.2        |
| Microsoft.WindowsAppSDK (inc. WinUI 3) | v1.2.230217.4 |

Find out more about [the template used for this project here](https://github.com/michael-hawker/WinUI2Plus3Template).

TODO: Update version numbers above if you update them locally.

## Linked Files

The following files are linked by default:

- App.xaml _(sets up the WinUI2/3 resources)_
- MainPage.xaml _(main place to write **your** code)_
- MainPage.xaml.cs _(main place to write **your** code)_
- Llama.jpg _(in case you need an image asset_ 🦙 _)_
- `Assets` Folder _(to reduce disk space)_

### Repro steps

TODO: Fill in these steps...

1. Change project to x64
2. Run ProjectTemplate.UWP
3. Click on button
4. See that it works...
5. Run ProjectTemplate.WinAppSDK
6. Click on button
7. See that issue X occurs...

You can see the code in the MainPage.xaml.cs on line ##...

----- TODO: Delete everything below after setting up your project. ----

## Using this template

We recommend performing a `git init` and `git commit` after creating this template so you can track the changes made to it. A `.gitignore` file has been provided already.

Remember to delete your `bin` and `obj` directories before zipping up your project for upload. You can use the provided `clean.bat` for that.

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
