# Solution dependency exe project outputs to main project directory with TargetFramework and not with TargetFrameworks

### Describe the bug

Prerequisites:

1. I have a project `A` that is an executable project; it depends on project `B`, which is also an executable project.
2. Since the sole purpose of project `B` is to execute during the compilation of project `A`, project `A` doesn't directly depend on `B` (i.e., it doesn't explicitly reference project `B` in the csproj file). Moreover, I don't expect project `B` to be output to the final output directory of project `A`.
3. The dependency between `A` and `B` is specified in the solution (sln) file.

Observations:

1. If the target framework of project `B` is set in singular form, then the `.exe`, `.deps.json`, and `.runtimeconfig.json` files of project `B` are output to the directory of project `A`. However, the `.dll` file is not. (This is an undesired outcome)
2. If the target framework of project `B` is set in plural form, then none of the files from project `B` are output to the directory of project `A`. (This is the desired outcome)

### To Reproduce

I've prepared a super simple reproducible project, which is uploaded on GitHub. You can try cloning it to reproduce.

In this project:

1. `Walterlv.IssueDemo.AppHostUnexpectedOutput`, which refers to project `A` mentioned above.
2. `Walterlv.IssueDemo.BeenDepended1`, which refers to project `B` mentioned above, with its target framework set in singular form.
3. `Walterlv.IssueDemo.BeenDepended2`, also referring to project `B`, but with its target framework set in plural form.

Upon trying to compile the solution, you'll notice that the output directory of `Walterlv.IssueDemo.AppHostUnexpectedOutput` contains `Walterlv.IssueDemo.BeenDepended1.exe`, but not `Walterlv.IssueDemo.BeenDepended2.exe`.

### Exceptions (if any)

No exceptions found.

### Further technical details

This issue occurs in the following SDK versions:

```powershell
> dotnet --list-sdks
7.0.400 [C:\Program Files\dotnet\sdk]
8.0.100-preview.7.23376.3 [C:\Program Files\dotnet\sdk]
```

Within the GitHub test project, I've included a global.json file. The issue appears both when using the preview version and when not using the preview version. These two tests correspond to the two different SDK versions mentioned above.
