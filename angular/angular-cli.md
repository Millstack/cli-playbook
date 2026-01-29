# Angular CLI Guide

Step-by-step, production-oriented reference for installing Angular CLI, creating projects, <br/> 
managing dependencies, configuring environments, and executing core development workflows.


## 1. System Prerequisites

### 1.1 Node version == Angular CLI version

The node LTS (*Long-Term Support*) version should fall within the required angular CLI range because of some factors:
* Stability guarantee :
  `Angular CLI depends on predictable Node APIs and behavior. LTS versions freeze breaking changes, so builds remain deterministic`
* Security compliance :
  `Non-LTS or EOL Node versions stop receiving security fixes. Angular blocks them to avoid insecure dev/runtime environments`
* Dependency compatibility :
  `Angular tooling (Webpack, Vite, esbuild, TypeScript, npm) targets LTS ranges. Outside that range, native bindings and package engines break`
* CI/CD reproducibility :
  `Enterprises standardize on LTS. Angular aligns with that to avoid pipeline failures across environments`

<br/>

> [!NOTE]
> check the intended angular CLI's version ranges, to choose correct node LTS version

```bash
npm view @angular/cli@17 engines
```

```html
@angular/cli@17.0.0 {
  npm: '^6.11.0 || ^7.5.6 || >=8.0.0',
  node: '^18.13.0 || >=20.9.0',
  yarn: '>= 1.13.0'
}
```

<br/>

### 1.2 Install Node.js (via nvm)

> NOTE: The nvm first needed to be installed, checkout [nvm guide](https://github.com/Millstack/cli-playbook/blob/main/angular/nvm.md)
<br/>

```bash
nvm install 20.0.0
nvm use 20.0.0
```

```bash
# to check the installed node and node package manager version
node -v
npm -v
```
<br/>

## 2. Install Angular CLI

to get the avaiable `angular CLI` versions
```bash
npm view @angular/cli versions --json
npm view @angular/cli versions --xml
```

to install only latest version of angular CLI
```bash
npm install -g @angular/cli
```
to install a specific version of angular CLI
```bash
npm install -g @angular/cli@17.0.0
```

<br/>

`Note:` @angular/cli only installs the **Command Line Interface** tool and give the ng `CLI executable binary` comamnd expose by package.

ng command is then use for:
  1. create new project : `ng new`
  2. serve app: `ng serve`
  3. build: `ng build`
  4. test: `ng test`
  5. manage updates: `ng update`
  6. check version: `ng version / ng --version`


to check the angular CLI version
```bash
ng --version
ng version
```

`NOTE:` to show the filesystem path of the ng executable installed by @angular/cli
```bash
# Windows
where ng
```
```bash
# macOS / Linux
which ng
```

`NOTE:` To remove the Angular CLI globally, which deletes the ng command from PATH
npm keeps only one global version at a time. Installing another version replaces the previous one.
```bash
npm uninstall -g @angular/cli
```
<br/>

## 3. Create New Angular App

```bash
ng new app-name
```
`options`
* Routing: Yes
* Styles: CSS / SCSS / Tailwind
* Server Side Rendering (SSR) & Static Site Generation (SSG): No (only for SEO, faster first paint or pre-rendered pages)

Minimal Enterprise Default Command
```bash
ng new my-app --style=css --routing --strict --skip-tests
```

<br/>

## 4. Install Project Dependencies

```bash
npm install
```

What npm install does:
* Downloads all dependencies listed in `package.json`
* Resolves versions using `package-lock.json`
* Creates/updates the `node_modules/ directory`
* Links executables into `node_modules/.bin`

<br/>

## 5. Add Angular Material

```bash
ng add @angular/material
```
<br/>

## 6. Creating Environment Files

environment files are essential for production / developement securities.

creating environemnt .ev files using `Windows PowerShell` commands
```bash
New-Item -ItemType Directory -Path src\environments
New-Item -ItemType File -Path src\environments\environment.ts
New-Item -ItemType File -Path src\environments\environment.prod.ts
```

creating environemnt files using `Linux / macOS` commands
```bash
mkdir -p src/environments
touch src/environments/environment.ts src/environments/environment.prod.ts
```

`NOTE:` basic environment files syntax

`environment.ts`
```bash
export const environment = {
  production: false,
  apiBaseUrl: 'https://localhost:7080/api/',
  
  // individual centralized service urls
  sidebarMenuUrl: 'v_1.0/SidebarMenu/GetSidebarMenu',
  getAllsidebarMenusUrl: 'v_1.0/SidebarMenu/GetAllMenus',
  userAuthenticateURL: 'v_1.0/Auth/Login',
  refreshTokenURL: 'v_1.0/Auth/refresh-token',
  saveRolesNRespURL: 'v_1.0/RolesAndResponsibility/save-rolesnresp',
};
```

<br/>

`environment.prod.ts`
```bash
export const environment = {
  production: true,
  apiBaseUrl: 'http://43.205.97.150:7080/api/',

  sidebarMenuUrl: 'v_1.0/SidebarMenu/GetSidebarMenu',
  getAllsidebarMenusUrl: 'v_1.0/SidebarMenu/GetAllMenus',
  userAuthenticateURL: 'v_1.0/Auth/Login',
  refreshTokenURL: 'v_1.0/Auth/refresh-token',
  saveRolesNRespURL: 'v_1.0/RolesAndResponsibility/save-rolesnresp',
};
```

<br/>

> [!NOTE]
> adding prod and dev environment files into the `angular.json` file, for switching to correct one, while using in development or production build

<br/>

```bash
"configurations": {
  "production": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.prod.ts"
      }
    ],
  }
}
```
<br/>

## 7. Common Code Generation Commands

7.1 `Generate Command Options`
```bash
# generate 4 files: componentname.html, componentname.ts, componentname.css & componentname.spec.ts
ng generate component foldername/componentname
```
```bash
# generate 3 files: componentname.html, componentname.ts & componentname.css
ng generate component foldername/componentname --skip-tests
```
```bash
# short form for generate component
ng g c foldername/componentname --skip-tests

# short form for generate service
ng g s services/auth

# short form for generate module
ng g m features/shared/models/CommonModels
```
```bash
# shows the intended outcome of the command in the console only, but no physical output
ng g c foldername/componentname --skip-tests --dry-run
```

<br/>

7.1 `Generate Component`
```bash
ng g c foldername/componentname --skip-tests
```

<br/>

7.2 `Generate Service`
```bash
ng g s core/auth/interceptors/auth.interceptor --skip-tests
```

<br/>

7.3 `Generate Module`
```bash
# --flat prevents angular CLI from creating a new folder for the generated file
ng g m features/shared/models/CommonModels --flat

features/shared/models/common-models.module.ts ✅
features/shared/models/CommonModels/common-models.module.ts ❌
```

<br/>

7.4 `Generate Guard`
```bash
ng g guard core/guards/auth --skip-tests
```
<br/>

7.5 `Generate Interceptor`
```bash
ng g interceptor auth
```
<br/>

7.6 `Generate Pipe`
```bash
ng generate pipe shared/capitalize
```

<br/>

7.7 `Generate Interfaces / Models`
```bash
ng g i models/order
```

<br/>

7.8 `Generate Enum`
```bash
ng generate enum models/status
```

<br/>

## 8. Dependency Management

```bash
npm install <package>
npm uninstall <package>
npm update
npm audit fix
```
<br/>

## 9. Angular CLI equivalent & Modern Flags
```bash
ng serve
ng build
ng build --configuration production
ng test
ng lint
```

`Serve`
```bash
ng serve
ng serve --open / -o
ng serve --hmr
ng serve --ssl
ng serve --port 4201
ng serve --host 0.0.0.0
ng serve --proxy-config proxy.conf.json
ng serve --configuration production
```
<br/>

`Build`
```bash
ng build
ng build --configuration production   // modern replacement for --prod
ng build --watch
ng build --source-map
ng build --stats-json
ng build --base-href /app/
ng build --deploy-url /cdn/
ng build --output-path dist/app
ng build --output-hashing all
```
<br/>

`Test`
```bash
ng test
ng test --watch=false
ng test --browsers=ChromeHeadless
ng test --code-coverage
```
<br/>

`Lint`
```bash
ng lint
ng lint --fix
```
<br/>

`Cache / Performance`
```bash
ng cache clean
ng build --no-cache
```
<br/>

`SSR / Advanced`
```bash
ng add @angular/ssr
ng serve --configuration development
ng build --configuration staging
```
<br/>

## 10. Clean-Up / Reset
```bash
# eliminate corrupted, stale, or incompatible dependency state
# if errors survive code changes, upgrades, or environment switches → reset dependency state
rm -rf node_modules package-lock.json
npm install
```
