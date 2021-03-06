# Fabric.SharedSettings

ReSharper and Code Analysis/StyleCop settings that we can share across the HealthCatalyst organization.

**Check out the [Wiki](https://github.com/HealthCatalyst/Fabric.SharedSettings/wiki) for suggestions on dealing with common error messages.**

## Getting Started ##

To begin, copy the `SharedSettings` folder from this repository into your solution's folder.

### Shared ReSharper settings ###

1. In Visual Studio, open your solution. Go to `Resharper > Manage Options`. Right-click the team-shared Solution settings layer (the middle one) and click `Add Layer > Open Settings File`. Select the `HealthCatalyst.Fabric.ReSharper.DotSettings` from the `SharedSettings` folder you copied to the solution folder. 
    > Note: If you have multiple solutions, consider picking one DotSettings file as the source of truth and having the rest of your solution DotSettings files inject that one as an extra layer. That way you don't have to work as hard to keep your various DotSettings files synchronized.
2. Commit changes to your repository. This should include both the new DotSettings file you copied, and some lines added to your team-shared solution DotSettings file that causes this new DotSettings file to be injected. It is safe to remove the AbsolutePath entry in the team-shared solution settings file, because you'll be relying on the path that's relative to your solution.
3. Open the SettingsFileComparisonTool.linq file in LINQPad. Change the paths in that file and run it to remove lines from your solution settings file that don't need to be there anymore.
4. Look at the remaining settings in your solution's team-shared settings file. If any of those settings are not specific to your code base, consider either dropping them to adopt the HealthCatalyst default, or submitting an issue and/or pull request to change the HealthCatalyst default to match your preference.

Now anybody who uses your solution will have this HealthCatalyst standard layer of style settings applied. Auto-formatting actions like `Ctrl-E, F` and `Ctrl-E, C` will apply these settings to your code styles.

### Code Analysis and StyleCop settings ###

There are a handful of Ruleset files in the `SharedSettings` folder. For each project, decide which one makes the most sense to use as your baseline.
   1. Catalyst.MostRules.Error.ruleset: This has most of Microsoft's recommended rules activated at Error level, which means that your build will fail if you're not following them.
   2. Catalyst.TestRules.ruleset: Similar to Catalyst.MostRules.Error.ruleset, but with some rules disabled which don't make sense for Test projects.
   3. Catalyst.ClassLibrary.ruleset: Similar to Catalyst.MostRules.Error.ruleset, but with a few changes which make sense for Class Library projects.

For each project:
1. Install the Microsoft.CodeAnalysis.FxCopAnalyzers NuGet package.
2. Create a RuleSet file that references one of the ruleset files you created above.
   1. Within Visual Studio, right-click the project. Go to Properties > Code Analysis. 
   2. Under "Configuration", select "All Configurations"
   3. Under "Run this rule set", select "Choose multiple rule sets..." 
   4. If the rule set file you wish to reference is not in the given list, click the "Add rule set" button at the top left. Otherwise, just select the rule set.
   5. "Save As..." to save the RuleSet file alongside your project's file in the solution. Choose a file name that corresponds with the name of your project file.
   6. "Enable Code Analysis on build" can be disabled with the new analyzers. Ensure "Suppress results from generated code" is enabled on your project.
3. Add the `StyleCop.Analyzers` NuGet package to your project. This will enable style warnings.

### EditorConfig

The `.editorconfig` file allows a development team to share consistent coding style rules between different editors and IDEs independent of platform.

To apply the shared `.editorconfig` settings to your project, move the `.editorconfig` file from the `SharedSettings` folder into the root folder of your repository. You can create other `.editorconfig` files in individual project folders to override these settings.

For more details, see https://editorconfig.org/

**Useful Resources:**

* [Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.EditorConfig)
* [Create portable, custom editor settings with EditorConfig](https://docs.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options)
* [.NET coding convention settings for EditorConfig](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference)

## Keeping Settings Updated ##

You can safely change your own Team-Shared ReSharper settings, as well as the project-specific `.ruleset` files in your solution to override these baseline settings with your own team's preferences. **Do not** change the `SharedSettings` files, or the root `.editorconfig` file in your own repository without getting them changed at the source, or your changes will be overwritten when a teammate updates these files someday.

When you make changes to ReSharper settings, Code Analysis/StyleCop rules, or `.editorconfig`, think carefully about whether they should be shared organization-wide. If so, check to see whether someone else has already added those changes to this repository: you might just need to copy the latest version of the files in `SharedSettings` into your folder again. If not, please contribute to this project to help others.

Periodically, you should update the files in your SharedSettings folder, as well as the root `.editorconfig` from the ones here.

## Contributing ##

Please use [the GitHub repository](https://github.com/HealthCatalyst/Fabric.SharedSettings) to suggest and make changes to these baseline settings file. Log an issue, create a pull request, comment on proposed changes with other developers, etc.
