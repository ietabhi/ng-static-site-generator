# ng-static-site-generator
[![Build Status](https://travis-ci.org/kevinphelps/ng-static-site-generator.svg?branch=master)](https://travis-ci.org/kevinphelps/ng-static-site-generator)
[![npm version](https://badge.fury.io/js/ng-static-site-generator.svg)](https://badge.fury.io/js/ng-static-site-generator)

`ng-static-site-generator` is a tool for building an [Angular](https://angular.io/) app and blog entries into a static html and css website. Building a client app to support dynamic functionality in the browser is also supported.

There is a [starter project](https://github.com/kevinphelps/ng-static-site-generator-starter) available. See [kevinphelps/kevinphelps.me](https://github.com/kevinphelps/kevinphelps.me) for another example.

## Features
- [x] Build an Angular app and blog entries into a static html and css website.
- [x] Build a client app to support dynamic functionality in the browser.
- [x] Watch build mode to automatically rebuild the site after changes.
- [x] Generate blog pages from source files written in markdown.
- [x] AOT build support for the client app to reduce bundle size.
- [ ] Server for testing the website when developing and writing blog entries. (`firebase serve` is a good alternative.)

## Installation

Clone [the starter project](https://github.com/kevinphelps/ng-static-site-generator-starter) to get started fast!

`yarn add [--exact] ng-static-site-generator` or `npm install --save-dev [--save-exact] ng-static-site-generator`

The following `peerDependencies` are required:

```json
{
  "dependencies": {
    "@angular/animations": ">4.0.0",
    "@angular/common": ">4.0.0",
    "@angular/core": ">4.0.0",
    "@angular/http": ">4.0.0",
    "@angular/platform-browser": ">4.0.0",
    "@angular/platform-server": ">4.0.0",
    "@angular/router": ">4.0.0",
    "reflect-metadata": ">0.1.0",
    "rxjs": ">5.0.0",
    "typescript": ">2.3.0",
    "zone.js": ">0.8.0"
  }
}
```

## CLI Commands

- `ng-static-site-generator build`: Builds the static site.
- `ng-static-site-generator build --prod`: Builds the static site for production (AOT compilation, minify js and html).
- `ng-static-site-generator watch`: Builds the static site and rebuilds after changes.

## Configuration

`ng-static-static-generator` is configured via a file named `ng-static-static-generator.json` at the root of the project.

```javascript
{
  "distPath": "./dist", // This is where the site will be generated.
  "blogPath": "./src/blog", // This is the folder where your blog entries are located.
  "stylesPath": "./src/styles.scss", // This is the file that contains your global styles.
  "templatePath": "./src/index.html", // This is your template html file. This is passed to HtmlWebpackPlugin.
  "appModule": "./src/app/app.module#AppModule", // This is the path and class name of your AppModule.
  "appRoutes": "./src/app/app-routing.module#routes", // This is the path and export name or your routes.
  "appComponent": "./src/app/app.component#AppComponent", // This is the path and name or your root component.

  // Options for building an optional client app.
  "mainPath": "./src/main.ts", // This is the file that contains the browser bootstrap code.
  "polyfillsPath": "./src/polyfills.ts" // Include this is you need a polyfills bundle.
}
```

## Using the NgStaticSiteGeneratorModule

`ng-static-static-generator` exposes functionality via the `NgStaticSiteGeneratorModule`.

```typescript
// app.module.ts
import { ModuleOptions, NgStaticSiteGeneratorModule } from 'ng-static-site-generator';

const ngStaticSiteGeneratorModuleOptions: ModuleOptions = {
  openExternalLinksInNewTab: false // Automatically add target="_blank" to external links. Default false.
};


@NgModule({
  imports: [
    ...
    NgStaticSiteGeneratorModule.forRoot(ngStaticSiteGeneratorModuleOptions)
  ],
  ...
})
export class AppModule { }

// my-component.component.ts
import { BlogService } from 'ng-static-site-generator';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.scss']
})
export class MyComponent {
  constructor(private blogService: BlogService) { }
}
```

## Blog entry source files

`ng-static-site-generator` uses jekyll-style files for blog entries. Files are placed in the `blogPath` folder specifed in `ng-static-static-generator.json`. (Note: Nesting folders within the blog path is not yet supported.)

- filename: `YYYY-MM-DD-url-slug.html` or `YYYY-MM-DD-url-slug.md` (e.g. `2017-06-26-this-is-a-blog-entry.html`)
- file contents: Metadata is given at the top of file delimited by lines containing `---`. Everything after the second `---` is body content written in html or markdown.

Example:

```markdown
---
title: This is the Title of the Blog Entry
description: This is a short description of the blog entry.
customProperty: This is a custom property. (Optional, of course.)
---

##This is the Title of the Blog Entry

This is the content of the blog entry.

<p>You can also write content in html if you want.</p>
```
