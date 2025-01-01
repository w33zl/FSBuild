# FSBuild - The build and management tool for Farming Simulator mods

This command line tool, **`fsbuild`**, assist you in managing your mod project and prepare your mod for submitting it to ModHub (and releases on other platforms).

```cmd
USAGE:
   fsbuild <command> [options]

COMMANDS:
   build        Build the mod - create a zip file from the mod folder
   test         Test the mod - create a zip file and run TestRunner
   release      Prepare for release - create a zip file, run TestRunner and (optionally) bump version info
   translate    Translate the mod - automatically translates the title, description and or the translation XML filesx
   help         Print the help text (you can also use --help with any command)
```

Each subcommand might have additional options, these can be printed using the `--help` flag, e.g. `fsbuild build --help`.

For more details on each subcommand, see separate sections below.

**Quick links:**
* [Installation instructions](#installation-instructions)
* [How to use FSBuild](#how-to-use-fsbuild)
* [Mod project configuration](#the-fsbuild-project-configuration-file-fsproj)

## Features

* Builds a zip archive from your mod folder (with support for blacklisting files to be excluded)
* Run TestRunner tool on your zip archive (this is important to ensure exactly the right files are included in the zip file)
* Translate title, description and/or all language_XX.xml files. This uses DeepL AI based automagic translations. Supports EN, DE and FR currently.

### Planned features
These features are planned, but it is not certain when (if ever) I actually make it to that point :) 

* Automatically bump major and minor mod version on release build
* Automatically install/update TestRunner tool
* Automatically bump modDesc version to latest known version
* Support for adding or removing prefixes to mod title and/or version to keep track of e.g. dev/preview versions (or GitHub specific releases)
* Tools for working with mod related images, e.g. converting to proper format for mod icon etc
* and more


## How to use FSBuild

> NOTE! TestRunner is currently disabled from both `test` and `release` until working properly.

### `build`
Compresses the mod folder into a zip archive ready for ModHub/distribution. Supports files/folder to be excluded via a blacklist. See the configuration settings for the `.fsproj` file below.

### `test`
Same as the `build` command, but also decompresses the zip archive and then run the TestRunner tool on these file. 

_It is important that this is an unzipped version of the zip archive and not the original mod folder since some files should be excluded from the zip archive._

### `release`
Same as the `test` command, but with additional features to automatically bump mod version and update modDesc version to latest known version. 

This build mode could also in the future apply additional rules to conform with ModHub or other specific changes desired release specific settigns (could e.g. be a flag `--preview` to generate a special mod title and mod version to indicate that is is a preview release). However, these features are not yet in place.

## Translate command
Requires a DeepL API key.
<TBD>

## The FSBuild project configuration file (*.fsproj*)

In essence, the .fsproj file is a JSON file looking like this:
```json
{
    "name": "FS25_ExampleMod",
    "sourceLanguage": "en",
    "excludeFiles": [ ],
}
```

The project configuration file is not needed to run the tool, if it is missing it will be created on the first run. The configuration file has the following items:

* **name:** The name of your mod, will be the name of the output zip archive
* **sourceLanguage:** The language to translate _from_. Currently supports EN, DE and FR. This means that if DE is the sourceLanguage, the translations will be in EN and FR.
* **excludeFiles:** An array of file and folder names to be excluded from the zip archive

_**Note:** The `.fsproj`, `fsbuild` and your zip file will be automatically added to the blacklist and is not needed in the 'excludeFiles' section._

## Installation instructions
Follow the instructions below to install FSBuild on your system. The only real mandatory step is the first one, the tool will work after that step (with limitations). However, to get a full installation it is recommended to also follow the optional steps 2-4:
1. **Unzip** the contents of the fsbuild.zip archive into any folder (from now on called _'installation folder'_ )
2. Add the path to the _installation folder_ in your **environment variables** (see [detailed instructions]() below)
3. Edit (or create) the `.env` file in the _installation folder_ to **configure FSBuild** (see configuration section below for details)
   1. Enable automatic translations with DeepL by providin an API key (it is easy and free)
   2. Enable automatic testing with TestRunner (two simple steps)
4. **Run the command** `fsbuild help` to ensure the installation is ok, if you see usage instrucitons everything is good

Note: If you don't want to change your environment variables, you can instead unzip the fsbuild.exe to your mod folder, however that is not recommended. You could also skip setting up environment variables and just use the full path each time you execute the fsbuild command (e.g. `C:\YourPath\fsbuild <command>`), while not convinient it works at least.

### Add environment variables (optional, recommended)
The benefit of adding your _installation folder_ to your environment variables is that the `fsbuild` command will be available in any folder and you can easily use it in any current or future mod folder. And as long as you replace the files in the same location on future updates of FSBuild, you don't need to ever change the environment variables, it will just work.

To add or change the environment variables for FSBuild, follow these steps:
1. Open your start menu and search for 'envir' and you should see an "Edit the system environment variables"
2. Click on the button "Environment Variables"
3. Now you can either add a new _user variable_ for FSBuild, which is the recommended method, or you can change the global _system variable_ `PATH`, which is not recommended
   * Add a _**User variable**_ (easiest but only works for current user):
      1.  Click the "New..." button below the list of _"User Variables for [NAME]"_
      2.  In the _Edit User Variable_ popup window, set the name to `FSBuild` and the _Variable Value_ to your _installation folder_ path (e.g. `C:\Program Files (x86)\Farming Simulator 2025\fsbuild`)
      3.  Click Ok
   *  Add a _**System variable**_ (higher risk of errors but works for all users):
      1.  Look in the list _"System Variables"_ for an item named `PATH`
      2.  Click the "Edit..." button
      3.  In the popup window, click the 
      4.  Click the "New..." button
      5.  Paste in the path to your _installation folder_ (e.g. `C:\Program Files (x86)\Farming Simulator 2025\fsbuild`)
      6.  Click Ok
      7.  Click Ok
   *  
4. After you have added a user or system variable, you need to logout of your system for the changes to have effect
5. Open a terminal window/command prompt, open any folder (except the _installation folder_) and try the `fsbuild` command.

ADD IMAGES


### Configuration (optional, recommended)
While these steps is also optional, it is highly recommended to take your time finishing these additional steps to use the full potential of FSBuild.

Open up your `.env` file from the _installation folder_ in a text editor, it should look like this:

```bash
VERBOSE=false
DEEPL_API_KEY="2f1e4b6a-0d7f-4c6a-9e4b-1234567890ab"
GAME_DIR="Q:\Games\Farming Simulator 25"
TEST_RUNNER_DIR="Q:\Games\FSTools\testRunner"
```

1. **Configure TestRunner**
   1. Download the latest TestRunner from Giants GDN webpage _(unfortunately, there is currentlyu noo way for me to automatically download it for you)_
   2. Unzip the contents of the TestRunner_public_X_Y_Z.zip file to any folder (e.g. `C:\Programs\testRunner`)
   3. Add the path from step 1.2 to the `TEST_RUNNER_DIR` variable in your _.env_ file (e.g. `TEST_RUNNER_DIR="C:\Programs\testRunner"`)
   4. Save the _.env_ file
2. **Configure automatic translation with DeepL**
   1. Go to the DeepL website https://www.deepl.com/pro-api
   2. Click the "Get started for free" button
   3. Choose the "Signup for free" below the "DeepL API Free" option
   4. Follow the instructions to create an free account
      - The free account has some limitations, however, what I could see this far this free account is well enough for our use cases
   5. After completing the signup process and validating your email, log in to your accout: https://www.deepl.com/en/your-account/subscription
   6. Go to the "API Keys" tab
   7. If there is no key in the list, create a new key with any name of your choice
   8. In the list of keys, choose one key to use for FSBuild
   9. In the column API key for that key, click the copy button right next to the text similar to this `9a1b******q:fx`
   10. Paste the copied API key into the `DEEPL_API_KEY` in your _.env_ file (e.g. `DEEPL_API_KEY="2f1e4b6a-0d7f-4c6a-9e4b-1234567890ab"`)
   11. Save the _.env_ file
3.  **Verbose logging**
    1.  If you want additional log items to be printed while using the tool, e.g. to better understand what is happening "under the hood" or if you need to debug issues with FSBuild, simply change 
4.  **Final step: Uncomment all changed variables**
    1.  As a final step, make sure you have uncommented all the variables that you have change, i.e. remove the leading `#` before the variable name, e.g. change `# VERBOSE=false` to `VERBOSE=false`
    2.  Save the _.env_ file


## Like the work I do?
I love to hear you feedback so please check out my [Facebook](https://www.facebook.com/w33zl). If you want to support me you can become my [Patron](https://www.patreon.com/wzlmodding) or buy me a [Ko-fi](https://ko-fi.com/w33zl) :heart:

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/X8X0BB65P) [![Support me on Patreon](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Fshieldsio-patreon.vercel.app%2Fapi%3Fusername%3Dwzlmodding%3F%26type%3Dpatrons&style=for-the-badge)](https://patreon.com/wzlmodding?)


