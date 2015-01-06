#NugetPackageRestore

MSBuild task to restore Nuget packages files to project after a Nuget package restore using Nuget.Core and Nuget.Commandline

##The Task
NugetPackageRestore creates a task that executes on BeforeBuild, it takes tacer of copying all of your package files to your project, by using the Nuget ProjectManager from Nuget.Core to reinstall your packages from your packages folder, right after Nuget package restore has restored all your packages. It requires to have the Nuget package restore feature enabled.

##The Background
Nuget package restore introduced in NuGet 2.7 allows to restore your packages to your solution:

- http://docs.nuget.org/docs/reference/package-restore

Altough this solution restores all packages, it does not copy package files to your solution folder, it leaves them in the packages folder. Such issue has been addressed several times in codeplex:

- http://nuget.codeplex.com/workitem/2094
- http://nuget.codeplex.com/workitem/1239

Jeff Handley explained the reason here:
- http://jeffhandley.com/archive/2013/12/09/nuget-package-restore-misconceptions.aspx

Other solutions to workaround this issue are available (this project is inspired by them):
- https://github.com/baseclass/Contrib.Nuget
- https://github.com/panchilo/MSBuild.NugetContentRestore
- https://github.com/uluhonolulu/BlogSamples/tree/master/NuGetSample

None of this solutions worked for me, I needed to be able to take advantage of Automatic Nuget Package Restore, and yet have all the downloaded packages automatically installed to my solution folder. So I came up with this task.

##Download
To install NugetPackageRestore, run the following command in the Package Manager Console:
    
	PM> Install-Package NugetPackageRestore
	
NuGet Package available at https://www.nuget.org/packages/NugetPackageRestore/

##Usage
After installing NugetPackageRestore a reference to NugetPackageRestore.targets will be added to your project, you can add attributes to the NugetPackageRestoreTask to modify its behavior (view NugetPackageRestore.targets for details):

SolutionDir - Full path to your solution

ProjectDir - Full path to your project

PackagesDir - Full path to packages folder

AddContentReferencesToProject - Enables to add content references of installed packages to your .csproj file, false by 
default

ConfigFileFullPath - Full path to packages config file, set to $(ProjectDir)packages.config by default

ProjectFileFullPath - Full path to project file, set to $(ProjectDir)*.csproj by default

##Debuggin the Task
In order to debug a task, a few easy things are required:
- First, right click on the Task Project and select **Properties**, on the window that opens select the **Debug** option.
- Once there, on the Start Action section select **Start external program** and enter the path for the appropriate MSBuild.exe, in my particular case I use windows 8 and my testing project uses .NET 4, so i used ***"C:\WINDOWS\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe"***. 
- Next, under Start Options, enter the name of your **.csproj file** in the **Command line arguments** textbox (***ie. "UnitTest.csproj"***) and the **full path to your .csproj file** in the **working directory** textbox (***ie. "C:\Users\deeplight77\Devel\Visual Studio Projects\NugetPackageRestore\UnitTest"***). 
- Then, **uncomment line 60** of **NugetPackageRestoreTask.cs** to launch the debugger and set a breakpoint at any point after this.
- Finally, all you have to do is **rebuild** the test project and presto! The Just-In-Time debugger will pop up asking you to select a debugger, select your test solution (in this case NugetPackageRestore) and you're ready to go.

##To DO
I am currently working on a way to automatically ignore all installed files from source control, beginning by adding them to .gitignore

##License
The MIT License (MIT)

Copyright (c) 2014, Alberto Rechy

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
