# Node Version Manager (NVM) Guide

Production-grade reference for installing, managing, validating, and locking Node.js versions using **nvm** across Windows, <br/> 
macOS, and Linux. Focused on deterministic builds, reproducible environments, and zero version drift.

---

## 1. What NVM Controls

- Manages **Node.js versions**
- Each Node version includes its own **npm**
- Global npm packages are scoped per Node version
- Switching Node = switching npm + globals

---

<br/>

## 2. NVM Installation

### For *Windows*
`Download Installer` via [github repository](https://github.com/coreybutler/nvm-windows/releases)

`verify` the downloaded nvm version
```bash
nvm version
```

### For *macOS / Linux*
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc   # or ~/.zshrc
nvm --version
```
<br/>

## 3. Node Installation

`install latest current node version`
```bash
nvm install node
```

`install latest LTS version`
```bash
nvm install --lts
```

`install specific version`
```bash
nvm install 20.17.0
```

`Check node version`
```bash
node -v
npm -v
```

<br/>

## 4. Use / Switch Node Versions

```bash
nvm use 20.17.0
nvm use --lts
nvm use node
```

<br/>

## 5. List Installed & Available Version

```bash
nvm list
nvm list available   # Windows only
nvm ls-remote        # macOS/Linux
```

<br/>

## 6. Uninstall Node Version

```bash
nvm uninstall 19.0.0
```

<br/>

## 7. Set Default Node Version

```bash
nvm alias default 20.17.0
```

`Verify the default set version`
```bash
nvm use default
```

<br/>

## 8. Lock Node Version Per Project (.nvmrc)

`Create`
```bash
echo 20.17.0 > .nvmrc
```

`Use`
```bash
nvm use
```

<br/>

## 9. Check Compatibility with Tooling

```bash
node -v
```

```bash
npm view @angular/cli@17 engines
```
```bash
@angular/cli@17.0.0 {
  npm: '^6.11.0 || ^7.5.6 || >=8.0.0',
  node: '^18.13.0 || >=20.9.0',
  yarn: '>= 1.13.0'
}
```

<br/>

> Note: if node version falls within angular CLI's version range, then the node version is valid for that angular project

<br/>

## 10. Remove Broken Environment
```bash
nvm uninstall 20.17.0
nvm install 20.17.0
nvm use 20.17.0
```

<br/>

## 11. Multiple Node Versions Per Angular Projects

Angular CLI has hard Node compatibility ranges. Each major Angular version only runs on specific Node majors. <br/>
Using the wrong Node breaks installs or runtime immediately. That is why multiple Node versions per machine are mandatory, not optional.

`Example:`
* Angular 15 → Node 14/16
* Angular 16 → Node 16/18
* Angular 17 → Node 18/20
* Angular 18 → Node 18/20+

<br/>

> [!WARNING]
> Projects created at different times lock different Angular majors. Upgrading Node globally breaks older projects.
> Upgrading Angular breaks older production pipelines.
> Therefore Node must be scoped per project.

<br/>

---

### Creation of .nvmrc file

```bash
cd projectname
echo 20 > .nvmrc
```

* `.nvmrc` stands for Node Version Manager Run Command / Run-time Configuration (informally) (common Unix convention)
* its just a plain text file that tells nvm which Node version a project “runs” on
* its only purpose: declare the Node version for that project so nvm use knows which version to activate
* inside each project, using this commands creates a hidden file `.nvmrc`, containg the node version value
* this `.nvmrc` file is mandatory, if we have to use `nvm use` for auto node version switching, or need to know explicit node version

---

### `.nvmrc` comes into picture

Each project declares its required Node version:
```bash
# specific node version representation for specific angular project
project-a/.nvmrc → 18
project-b/.nvmrc → 20
```

`Meaning`
* Project A was built on Angular 16 → requires Node 18
* Project B uses Angular 17 → requires Node 20
* Inside each project there is a hidde file --> .nvmrc
* .nvmrc file contains the node version for that project

`Steps:`

```bash
cd project-a
```
* enters into the project file

```bash
nvm use
```
* nvm reads the .nvmrc file
* switches shell to that node version
* if missing, then prompts for installing it

```bash
ng serve -o
```
* angular sees correct node version --> runs
* if node mismatches, then angular CLI exists with an error
* if node matches, then angualr CLI serves or builds successfully

`No conflicts. No reinstalls. No breakage.`

---

### Switching Into Explicit Node Version

```bash
cd project
nvm use 20
```
* Manually forces Node version to 20
* Ignores `.nvmrc` file
* Works only if you already know the correct version
* `Production Rule` every node project must contain `.nvmrc` file
* Never rely on humans remembering versions
* Never hardcode Node in docs


