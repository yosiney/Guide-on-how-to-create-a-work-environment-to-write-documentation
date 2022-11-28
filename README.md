## Guide on how to create a work environment to write documentation

With the next tools: 

- Sistema Operativo: Ubuntu 22.04 (no usar snaps)
- VSCode
- https://vale.sh/  (Linter)
- https://valentjn.github.io/ltex/ 
- https://prettier.io/ (Formatter) 
- LanguageTool (Extensiones para navegadores)

## Table of Contents
 - [Vale](#vale)
 - [vscode-ltex](#vscode-ltex)
 - [Prettier](#prettier)
 - [LanguageTool](#languagetool)

Taking into account that we already have what is ubuntu 22.04 and VSCode installed, we go to the next step, which is the installation of:



# Vale

### Installation
Get started with Vale, a syntax-aware linter for prose built with speed and extensibility in mind.

### Package Managers 
In general, the recommended approach on all platforms is to use a package manager such as Homebrew (macOS) or Chocolatey (Windows):

``` bash
snap install --edge vale
```

See Snapcraft for more information.

``` bash
brew install vale
```
See Linuxbrew for more information.
On Fedora, you can use the mczernek/vale COPR repository:

``` bash
sudo dnf copr enable mczernek/vale && sudo dnf install vale
```
This will ensure that Vale is available on your $PATH and allow you to stay up to date with new releases.



## GitHub Releases

Archives of precompiled binaries are available for Windows, macOS, and Linux. To use one of these, you’ll need to download the archive for your platform, extract it to a local directory, and (optionally) add the extracted directory to your $PATH.

An example of this process for Linux is given below:

``` bash
wget https://github.com/errata-ai/vale/releases/download/v2.15.4/vale_2.15.4_Linux_64-bit.tar.gz
mkdir bin && tar -xvzf vale_2.15.4_Linux_64-bit.tar.gz -C bin
export PATH=./bin:"$PATH"
```

## Docker 

Vale is available on Docker Hub at jdkato/vale:

``` bash
docker pull jdkato/vale
```

Vale requires three components: a .vale.ini config file, a StylesPath directory (specified in the config file), and a document or directory to lint.

Here’s an example of calling Vale with locally-defined components (assuming $(pwd)/fixtures/styles/demo contains a config file):

``` docker
docker run --rm -v $(pwd)/styles:/styles \
           --rm -v $(pwd)/fixtures/styles/demo:/docs \
           -w /docs \
           jdkato/vale .
```

By default, the image supports HTML, Markdown, AsciiDoc, and reStructuredText content. If you need support for DITA as well, you’ll need to add the relevant dependencies—for example,

``` docker
# Choose a version to pin:
FROM jdkato/vale:v2.15.2

# Copy a local installation of the DITA Open Toolkit:
COPY bin/dita-ot-3.6 /
ENV PATH="/dita-ot-3.6/bin:$PATH"

ENTRYPOINT ["/bin/vale"]
```


# Vale + VS Code

> The official Visual Studio Code extension for [Vale](https://github.com/errata-ai/vale).

The Vale extension for VS Code provides customizable spelling, style, and grammar checking for a variety of markup formats (Markdown, AsciiDoc, reStructuredText, HTML, and DITA).

As of **v0.15.0**, the extension drops support for [Vale Server](https://errata.ai/vale-server/) which has ceased development. Many of the features from Vale Server will find their way into the Vale CLI tool, and this extension.

## Installation

1. Install [Vale](https://docs.errata.ai/vale/install);
2. install `vale-server` (this extension) via the [Marketplace](https://marketplace.visualstudio.com/items?itemName=errata-ai.vale-server);
3. restart VS Code (recommended).

## Features

### Detailed Problems View

<p align="center">
  <img src="https://user-images.githubusercontent.com/8785025/89956665-76c9fa80-dbea-11ea-9eba-3f272a5a26e5.png" />
</p>

Browse detailed information for each alert, including the file location, style, and rule ID.

### Go-To Rule

<p align="center">
  <img src="https://user-images.githubusercontent.com/8785025/89956857-d1635680-dbea-11ea-8e50-8e2715721e5d.png" />
</p>

Navigate from an in-editor alert to a rule's implementation on your `StylesPath` by clicking "View Rule".

### Quick Fixes

<p align="center">
  <img src="https://user-images.githubusercontent.com/8785025/89957413-2eabd780-dbec-11ea-97e1-9a04bce950ce.png" />
</p>

Fix word usage, capitalization, and more using [Quick Fixes](https://code.visualstudio.com/docs/editor/refactoring#_code-actions-quick-fixes-and-refactorings) (macOS: <kbd>cmd</kbd> + <kbd>.</kbd>, Windows/Linux: <kbd>Ctrl</kbd> + <kbd>.</kbd>). The quick fixes feature depends on the underlying rule implementing an action that VSCode can then trigger.

Spelling errors are currently not supported, but will be supported in a future version.

## Settings

The extension offers a number of settings and configuration options (_Preferences > Extensions > Vale_).

- `vale.valeCLI.config` (default: `null`): Absolute path to a Vale configuration file. Use the predefined [`${workspaceFolder}`](https://code.visualstudio.com/docs/editor/variables-reference#_predefined-variables) variable to reference configuration file from a custom location. (**NOTE**: On Windows you can use '/' and can omit `.cmd` in the path value.) If not specified, the extension uses the default search process (relative to the current file).

    **Example**

    ```jsonc
    {
      // You can use ${workspaceFolder} it will be replaced by workspace folder path
      "vale.valeCLI.config": "${workspaceFolder}/node_modules/some-package/.vale.ini"

      // or use some absolute path
      "vale.valeCLI.config": "/some/path/to/.vale.ini"
    }
    ```

- `vale.valeCLI.path` (default: `null`): Absolute path to the Vale binary. Use the predefined [`${workspaceFolder}`](https://code.visualstudio.com/docs/editor/variables-reference#_predefined-variables) variable to reference a non-global binary. (**NOTE**: On Windows you can use '/' and can omit `.cmd` in the path value.)

    **Example**

    ```jsonc
    {
      // You can use ${workspaceFolder} it will be replaced by workspace folder path
      "vale.valeCLI.path": "${workspaceFolder}/node_modules/.bin/vale"

      // or use some absolute path
      "vale.valeCLI.path": "/some/path/to/vale"
    }
    ```

- `vale.valeCLI.minAlertLevel` (default: `inherited`): Defines from which level of errors and above to display in the problems output.


When we have finished installing and integrating the tool vale to VSCode, we go to the next tool, which would be:

# vscode-ltex

## Features

![Grammar/Spell Checker for VS Code with LanguageTool and LaTeX Support](https://github.com/valentjn/vscode-ltex/raw/release/img/banner-ltex.png)

- **Supported markup languages:** BibT<sub>E</sub>X, ConT<sub>E</sub>Xt, L<sup>A</sup>T<sub>E</sub>X, Markdown, Org, reStructuredText, R Sweave, XHTML
- Comment checking in **many popular programming languages** (optional, opt-in)
- Comes with **everything included,** no need to install Java or LanguageTool
- **Offline checking:** Does not upload anything to the internet
- Supports **over 20 languages:** English, French, German, Dutch, Chinese, Russian, etc.
- **Issue highlighting** with hover description
- **Replacement suggestions** via quick fixes
- **User dictionaries**
- **Multilingual support** with babel commands or magic comments
- Possibility to use **external LanguageTool servers**
- **[Extensive documentation][website]**

## Requirements

<!-- #if TARGET == 'vscode' -->
- 64-bit Linux, Mac, or Windows operating system
- [VS Code 1.52.0 or newer](https://code.visualstudio.com/)
- Optional:
  - If you want to check documents written in a markup language that VS Code does not support out-of-the-box (e.g., L<sup>A</sup>T<sub>E</sub>X), install an extension that provides support for that language (e.g., [LaTeX Workshop Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)) in addition to this extension.
<!-- #elseif TARGET == 'coc.nvim' -->
<!-- - 64-bit Linux, Mac, or Windows operating system -->
<!-- - [Node.js 14.16.0 or later](https://nodejs.org/) -->
<!-- - [Vim](https://www.vim.org/) or [Neovim](https://neovim.io/) with [coc.nvim 0.0.80 or newer](https://github.com/neoclide/coc.nvim) -->
<!-- #endif -->

## How to Use

<!-- #if TARGET == 'vscode' -->
1. Install the requirements listed above
2. Install this extension (see [download options](https://valentjn.github.io/ltex/vscode-ltex/installation-usage-vscode-ltex.html#how-to-install-and-use))
3. Reload the VS Code window if necessary
4. Open a L<sup>A</sup>T<sub>E</sub>X or a Markdown document, or open a new file and change the language mode to `LaTeX` or `Markdown` (open the Command Palette and select `Change Language Mode`)
5. Wait until [ltex-ls](https://valentjn.github.io/ltex/faq.html#whats-the-difference-between-vscode-ltex-ltex-ls-and-languagetool) has been found; if necessary, LT<sub>E</sub>X downloads it for you. Alternatively, you can choose [offline installation](https://valentjn.github.io/ltex/vscode-ltex/installation-usage-vscode-ltex.html#offline-installation).
6. Grammar/spelling errors will be displayed! (if there are any)
<!-- #elseif TARGET == 'coc.nvim' -->
<!-- 1. Install the requirements listed above -->
<!-- 2. Install coc-ltex by running `:CocInstall coc-ltex` -->
<!-- 3. If you want to check LaTeX documents: Add `let g:coc_filetype_map = {'tex': 'latex'}` to `~/.vimrc` (Vim) or `~/.config/nvim/init.vim` (workaround for [#425](https://github.com/valentjn/vscode-ltex/issues/425), until [neoclide/coc.nvim#3433](https://github.com/neoclide/coc.nvim/pull/3433) is released) -->
<!-- 4. Open a LaTeX or a Markdown document -->
<!-- 5. Wait until [ltex-ls](https://valentjn.github.io/ltex/faq.html#whats-the-difference-between-vscode-ltex-ltex-ls-and-languagetool) has been downloaded and started -->
<!-- 6. Grammar/spelling errors will be displayed! (if there are any) -->
<!-- #endif -->

## Installation and Usage (vscode-ltex)

#### Download Providers

- Download from within VS Code: Open the Extensions panel and type ltex
- [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=valentjn.vscode-ltex) (VS Code uses this for downloading)
- [Open VSX](https://open-vsx.org/extension/valentjn/vscode-ltex)
- [Releases on GitHub](https://github.com/valentjn/vscode-ltex/releases) (+ packages for offline installation)
-  [Source on GiHub](https://github.com/valentjn/vscode-ltex)

## Requirements
- 64-bit Linux, Mac, or Windows operating system
- VS Code 1.52.0 or later (new versions of LTEX released on or after January 14, 2022, will require VS Code 1.61.0 or later)
- Optional:
   - If you want to check LATEX documents: [LaTeX Workshop Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
   - If you want to check Org documents: [Org Mode Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=tootone.org-mode)
   - If you want to check reStructuredText documents: [reStructuredText Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=lextudio.restructuredtext)

### How to Install and Use

1. Install the requirements listed above
2. Install this extension
3. Reload the VS Code window if necessary
4. Open a LATEX or a Markdown document, or open a new file and change the language   mode to LATEX or Markdown (open the Command Palette and select **Change Language Mode**)
5. Wait until [ltex-ls](https://valentjn.github.io/ltex/faq.html#whats-the-difference-between-vscode-ltex-ltex-ls-and-languagetool) has been found; if necessary, LTEX downloads it for you. Alternatively, you can choose [offline installation.](https://valentjn.github.io/ltex/vscode-ltex/installation-usage-vscode-ltex.html#offline-installation)
Grammar/spelling errors will be displayed! (if there are any)

### Offline Installation
The key component of LTEX, the LTEX Language Server (ltex-ls), which also includes Java and LanguageTool, cannot be included in the LTEX version distributed on the Visual Studio Marketplace due to file size restrictions.

When activated for the first time, LTEX will automatically download and use a hard-coded ltex-ls release from GitHub. It will be stored in the extension folder. Note that after this initial installation, no connection to the Internet is necessary. Checking your documents is completely offline.

If you don’t have an Internet connection, or if you simply don’t want this, there are two alternatives.

In case there are any problems, you will find additional debug info in **View** → **Output** → **LTeX Language Server** and **LTeX Language Client** 

#### First Alternative: Download the Offline Version of LTEX
Download the offline version of LTEX at the [Releases page on GitHub](https://github.com/valentjn/vscode-ltex/releases) and install it via **Extensions: Install from VSIX...** on the Command Palette. The offline version already includes ltex-ls (and the portable Java distribution). Reload the Visual Studio Code window after installing the offline version.

#### Second Alternative: Download ltex-ls/Java Manually
Download [ltex-ls](https://github.com/valentjn/ltex-ls/releases) and/or a Java distribution (e.g., [Eclipse Adoptium](https://adoptium.net/)) individually and set [ltex.ltex-ls.path](https://valentjn.github.io/ltex/settings.html#ltexltex-lspath) and/or [ltex.java.path](https://valentjn.github.io/ltex/settings.html#ltexjavapath) to the respective locations. If you download a binary release of ltex-ls (those include a platform in the archive file name), then that release already includes Java and you don’t need to set [ltex.java.path.](https://valentjn.github.io/ltex/settings.html#ltexjavapath)

Note that the versions of ltex-ls and/or Java have to satisfy the following requirements:

 - Each version of LTEX has been tested with exactly one version of ltex-ls, which is the version that LTEX automatically downloads. If you download ltex-ls manually, be sure to use this version of ltex-ls. An older or a newer version of ltex-ls might work, or it might not.

 For this reason, this approach is not recommended as automatically updated versions of LTEX (by Visual Studio Code) might not be compatible anymore with your manually downloaded version of ltex-ls. If you choose this approach, remember to update ltex-ls if LTEX doesn’t work anymore.

 To find out which version a particular version of LTEX uses, check the [changelog](https://valentjn.github.io/ltex/vscode-ltex/changelog.html) for **Update LTEX LS to X.Y.Z.** If there is no entry of this form in the changelog for the version of LTEX you want to use, use the entry of the first previous version of LTEX that has such an entry.

 - The version of Java must be at least 11. Some Java distributions offer a JRE (Java Runtime Environment) and a JDK (Java Development Kit); in this case, the JRE is sufficient.

If you download Java, you can also decide to install it system-wide. In this case, LTEX should be able to automatically detect its location. If not, you can still set [ltex.java.path](https://valentjn.github.io/ltex/settings.html#ltexjavapath) to the location of your system-wide installation.

Reload the Visual Studio Code window after installing ltex-ls and/or Java.


# Prettier

First, install Prettier locally:

<!--DOCUSAURUS_CODE_TABS-->
<!--npm-->

```bash
npm install --save-dev --save-exact prettier
```

<!--yarn-->

```bash
yarn add --dev --exact prettier
```

<!--END_DOCUSAURUS_CODE_TABS-->

Then, create an empty config file to let editors and other tools know you are using Prettier:

<!-- Note: `echo "{}" > .prettierrc.json` would result in `"{}"<SPACE>` on Windows. The below version works in cmd.exe, bash, zsh, fish. -->

```bash
echo {}> .prettierrc.json
```

Next, create a [.prettierignore](ignore.md) file to let the Prettier CLI and editors know which files to _not_ format. Here’s an example:

```
# Ignore artifacts:
build
coverage
```

> Tip! Base your .prettierignore on .gitignore and .eslintignore (if you have one).

> Another tip! If your project isn’t ready to format, say, HTML files yet, add `*.html`.

Now, format all files with Prettier:

<!--DOCUSAURUS_CODE_TABS-->
<!--npm-->

```bash
npx prettier --write .
```

> What is that `npx` thing? `npx` ships with `npm` and lets you run locally installed tools. We’ll leave off the `npx` part for brevity throughout the rest of this file!
>
> Note: If you forget to install Prettier first, `npx` will temporarily download the latest version. That’s not a good idea when using Prettier, because we change how code is formatted in each release! It’s important to have a locked down version of Prettier in your `package.json`. And it’s faster, too.

<!--yarn-->

```bash
yarn prettier --write .
```

> What is `yarn` doing at the start? `yarn prettier` runs the locally installed version of Prettier. We’ll leave off the `yarn` part for brevity throughout the rest of this file!

<!--END_DOCUSAURUS_CODE_TABS-->

`prettier --write .` is great for formatting everything, but for a big project it might take a little while. You may run `prettier --write app/` to format a certain directory, or `prettier --write app/components/Button.js` to format a certain file. Or use a _glob_ like `prettier --write "app/**/*.test.js"` to format all tests in a directory (see [fast-glob](https://github.com/mrmlnc/fast-glob#pattern-syntax) for supported glob syntax).

If you have a CI setup, run the following as part of it to make sure that everyone runs Prettier. This avoids merge conflicts and other collaboration issues!

```bash
npx prettier --check .
```

`--check` is like `--write`, but only checks that files are already formatted, rather than overwriting them. `prettier --write` and `prettier --check` are the most common ways to run Prettier.

## Set up your editor

Formatting from the command line is a good way to get started, but you get the most from Prettier by running it from your editor, either via a keyboard shortcut or automatically whenever you save a file. When a line has gotten so long while coding that it won’t fit your screen, just hit a key and watch it magically be wrapped into multiple lines! Or when you paste some code and the indentation gets all messed up, let Prettier fix it up for you without leaving your editor.

See [Editor Integration](editors.md) for how to set up your editor. If your editor does not support Prettier, you can instead [run Prettier with a file watcher](watching-files.md).

> **Note:** Don’t skip the regular local install! Editor plugins will pick up your local version of Prettier, making sure you use the correct version in every project. (You wouldn’t want your editor accidentally causing lots of changes because it’s using a newer version of Prettier than your project!)
>
> And being able to run Prettier from the command line is still a good fallback, and needed for CI setups.

## ESLint (and other linters)

If you use ESLint, install [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier#installation) to make ESLint and Prettier play nice with each other. It turns off all ESLint rules that are unnecessary or might conflict with Prettier. There’s a similar config for Stylelint: [stylelint-config-prettier](https://github.com/prettier/stylelint-config-prettier)

(See [Prettier vs. Linters](comparison.md) to learn more about formatting vs linting, [Integrating with Linters](integrating-with-linters.md) for more in-depth information on configuring your linters, and [Related projects](related-projects.md) for even more integration possibilities, if needed.)

## Git hooks

In addition to running Prettier from the command line (`prettier --write`), checking formatting in CI, and running Prettier from your editor, many people like to run Prettier as a pre-commit hook as well. This makes sure all your commits are formatted, without having to wait for your CI build to finish.

For example, you can do the following to have Prettier run before each commit:

1. Install [husky](https://github.com/typicode/husky) and [lint-staged](https://github.com/okonet/lint-staged):

   <!--DOCUSAURUS_CODE_TABS-->
   <!--npm-->

   ```bash
   npm install --save-dev husky lint-staged
   npx husky install
   npm pkg set scripts.prepare="husky install"
   npx husky add .husky/pre-commit "npx lint-staged"
   ```

   <!--yarn-->

   ```bash
   yarn add --dev husky lint-staged
   npx husky install
   npm pkg set scripts.prepare="husky install"
   npx husky add .husky/pre-commit "npx lint-staged"
   ```

   > If you use Yarn 2, see https://typicode.github.io/husky/#/?id=yarn-2

   <!--END_DOCUSAURUS_CODE_TABS-->

2. Add the following to your `package.json`:

```json
{
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
}
```

> Note: If you use ESLint, make sure lint-staged runs it before Prettier, not after.

See [Pre-commit Hook](precommit.md) for more information.

## Summary

To summarize, we have learned to:

- Install an exact version of Prettier locally in your project. This makes sure that everyone in the project gets the exact same version of Prettier. Even a patch release of Prettier can result in slightly different formatting, so you wouldn’t want different team members using different versions and formatting each other’s changes back and forth.
- Add a `.prettierrc.json` to let your editor know that you are using Prettier.
- Add a `.prettierignore` to let your editor know which files _not_ to touch, as well as for being able to run `prettier --write .` to format the entire project (without mangling files you don’t want, or choking on generated files).
- Run `prettier --check .` in CI to make sure that your project stays formatted.
- Run Prettier from your editor for the best experience.
- Use [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) to make Prettier and ESLint play nice together.
- Set up a pre-commit hook to make sure that every commit is formatted.


We would only have what it is to integrate the last tool that would be:


# LanguageTool

### how install with Chrome?

1. Enter in: [Grammar & Spell Checker — LanguageTool](https://chrome.google.com/webstore/detail/grammar-spell-checker-%E2%80%94-l/oldceeleldhonbafppcapldpdifcinji)
2. And press where it says add to chrome

### how install with Firefox?

1. Enter in: [Grammar & Spell Checker — LanguageTool](https://addons.mozilla.org/es/firefox/addon/languagetool/)
2. And press where it says add to Firefox




