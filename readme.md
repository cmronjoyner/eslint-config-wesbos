# No-Sweat™ Eslint and Prettier Setup

These are my settings for ESLint and Prettier

You might like them - or you might not. Don't worry you can always change them.

## What it does

- Lints JavaScript based on the latest standards
- Fixes issues and formatting errors with Prettier
- Lints + Fixes inside of html script tags
- Lints + Fixes React via eslint-config-airbnb
- You can see all the [rules here](https://github.com/wesbos/eslint-config-wesbos/blob/master/.eslintrc.js) - these generally abide by the code written in my courses. You are very welcome to overwrite any of these settings, or just fork the entire thing to create your own.

## Installing

You can use eslint globally and/or locally per project.

It's usually best to install this locally once per project, that way you can have project specific settings as well as sync those settings with others working on your project via git.

I also install globally so that any project or rogue JS file I write will have linting and formatting applied without having to go through the setup. You might disagree and that is okay, just don't do it then 😃.

With either option, we will need to use the package [install-peerdeps](https://www.npmjs.com/package/install-peerdeps) to automatically install peer dependency packages.

```console
npm install -g install-peerdeps
```

## Local / Per Project Install - Manual

1. If you don't already have a `package.json` file, create one with `npm init`.

2. Then we need to install everything needed by the config:

```console
npx install-peerdeps --dev eslint-config-wesbos
```

3. You can see in your package.json there are now a big list of devDependencies.

4. Create a `.eslintrc` file in the root of your project's directory (it should live where package.json does). Your `.eslintrc` file should look like this:

```json
{
  "extends": ["wesbos"]
}
```

Tip: You can alternatively put this object in your `package.json` under the property `"eslintConfig":`. This makes one less file in your project.

5. You can add two scripts to your package.json to lint and/or fix:

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
},
```

6. Now you can manually lint your code by running `npm run lint` and fix all fixable issues with `npm run lint:fix`. You probably want your editor to do this though.

## Local / Per Project Install - Automatic

Here we will create a shell script to automate creation of your local install

```console
# Initialize NPM Project as needed
if [ ! -f package.json ]; then
  npm init -y
fi
# Save any prior copy of .eslintrc
if [ -f .eslintrc ]; then
  mv ./.eslintrc ./old.eslintrc
  echo "\nSuccess: Your prior .eslintrc file has been renamed to old.eslintrc"
fi
# Create New .eslintrc
echo $'{\n  "extends": ["wesbos"]\n}' > ./.eslintrc
echo "\nNew .eslintrc file has been created."
# Install Wes eslint settings
npx install-peerdeps --dev eslint-config-wesbos
echo "\nSuccess: Your project has been initialized with a Wes' eslint configuration"
```

1. Copy this code, and save it in an appropriate place under the filename "wbeslint.sh".

2. Make the wbeslint.sh file executable.

```console
chmod +x wbeslint.sh
```

3. Also consider appending an alias in your preferred shell config (something like ~/.zshrc or ~/.bashrc) so you can forget about the path. After adding it, the alias won't work in this shell window, so start a new one.

```
alias wbeslint='sh ~/path/to/the/wbeslint.sh'
```

4. Now within the main project directory, simply type "wbeslint" and see the show.

## Global Install

1. First install everything needed:

```
npx install-peerdeps --global eslint-config-wesbos
```

(**note:** npx is not a spelling mistake of **npm**. `npx` comes with when `node` and `npm` are installed and makes script running easier 😃)

2. Then you need to make a global `.eslintrc` file:

ESLint will look for one in your home directory

- `~/.eslintrc` for mac
- `C:\Users\username\.eslintrc` for windows

In your `.eslintrc` file, it should look like this:

```json
{
  "extends": ["wesbos"]
}
```

3. To use from the CLI, you can now run `eslint .` or configure your editor as we show next.

## With VS Code

Once you have done one, or both, of the above installs. You probably want your editor to lint and fix for you. Here are the instructions for VS Code:

1. Install the [ESLint package](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
2. Now we need to setup some VS Code settings via `Code/File` → `Preferences` → `Settings`. It's easier to enter these settings while editing the `settings.json` file, so click the `{}` icon in the top right corner:

```js
  // These are all my auto-save configs
"editor.formatOnSave": true,
// turn it off for JS, we will do this via eslint
"[javascript]": {
  "editor.formatOnSave": false
},
// tell the ESLint plugin to run on save
"eslint.autoFixOnSave": true,
// Optional BUT IMPORTANT: If you have the prettier extension enabled for other languages like CSS and HTML, turn it off for JS since we are doing it through Eslint already
"prettier.disableLanguages": [
  "js"
],
```

## 🤬🤬🤬🤬 ITS NOT WORKING

start fresh. Sometimes global modules can goof you up. This will remove them all.

```
npm remove --global eslint-config-wesbos babel-eslint eslint eslint-config-prettier eslint-config-airbnb eslint-plugin-html eslint-plugin-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier
```

To do the above for local, omit the `--global` flag.

Then if you are using a local install, remove your `package-lock.json` file and delete the `node_modules/` directory.

Then follow the above instructions again.
