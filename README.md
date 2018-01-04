# Shape Books a TakeShape Demo
This is a demo project to help get you started building a static website with TakeShape (TS). You can see the demo project in action here: http://shapebooks.s3-website-us-east-1.amazonaws.com/


## Quickstart
*If you're using JetBrains we recommend using the  [JS GraphQL plugin](https://github.com/jimkyndemeyer/js-graphql-intellij-plugin). This plugin allows for real time validation of your queries against the schema you define in TS though modeling*
 
1. On TS set up a project and then configure a static site 
2. `git clone https://github.com/takeshape/takeshape-demo [YOUR-PROJECT-NAME]`
3. `cd` to [YOUR-PROJECT-NAME]
4. `yarn install` - This will install all dependencies including the takeshape-cli `tsg` (If you want to install TS CLI globally `npm install -g takeshape-cli`)
5. `yarn add takeshape-cli` Make sure you're using the latest version of takeshape-cli 
6. `yarn run init` - Follow the command prompts to set up your local environment to communicate with TS
7. `yarn run start` -  The server runs on [http://localhost:5000](http://localhost:5000) by default
8. `yarn run deploy` - Deploy your site to TS!

## Project Structure
Files and directories in this repo are designed to get you up and running with TS in seconds. After you poke around a bit you'll see how easy it is to lay in your own build processes and tools. One of the core concepts of TS is to do just enough and provide just enough structure to make your life easier without getting in your way. We want you to be creative. 

### Directories
- `src` - Holds template files (configurable by `templatePath`) that will be processed by the TS static site generator or files and directories that will be handled by your own build process. 
- `static` - (configurable in tsg.yml) Anything inside the static folder will be copied over into the build folder. If you have a build process to generate files like scss into css or combine js files they should output into the static folder or in subdirectories of the static folder.
   
### TS Configuration
- `.graphqlrc` - Generated by `init`. Reference to the TS endpoint
- `.tsgrc` -  Generated by `init`.  Details to connect to a specific project on the TS platform
- `tsg.yml` - Configuration options to build your project including paths to relevant directories, routes and data contexts

#### `tsg.yml` Configuration Options

```
templatePath: src/templates   #Sets the path to look for templates
staticPath: static            #TS deploys this directory. All of your JS, CSS need to end up here. Files like robots.txt, humans.txt and other files that do not need processing should live here.
buildPath: build              #Temporary build directory
 
 locale: en-us #defaut
 dates:
   tz: America/New_York #default
   format: LLL #default

context:                      #Global context available to all routes. 
  assets: ../../static/assets/manifest.json
  [KEY]: [VALUE]
  ...
  
routes:                       #Routes tell TS which template to join with which context (graphql query) and the format of the resulting directory structure to generate
  homepage:
    path: /
    template: pages/homepage.html
    context: data/homepage.graphql
...    
 ```

### General Configuration 
- `.gitignore` - What files to ignore if you're using git (you should use some form of version control, if you aren't you're doing it wrong).
- `.eslintrc` - JS linting
- `.eslintrc` - JS linting
- `.stylelinerc` - CSS linting rules [https://github.com/stylelint/stylelint](https://github.com/stylelint/stylelint).
- `postcss.config.js` - Basic config for postcss.
- `.editorcconfig` - Maintain a consistent coding style [http://editorconfig.org/](https://github.com/stylelint/stylelint).
- `webpack.config.js` - Compile JS and CSS and provide cache busting
 
### Dependency Management
- `yarn.lock` -  Generated through`yarn install`
- `package.json` - Serves two purposes, identifies the packages that will be installed when you run `yarn install`. Secondly establishes the various commands that can be executed by `yarn run` ex- `yarn run init`, or `yarn run start`, and `yarn run deploy`. You can add your own commands here to create your own build process. Or you can use a completely different build system like gulp or grunt.   

## Templating
TS uses the Nunjucks templating language. You can find detailed documentation on the Nunjucks site: (https://mozilla.github.io/nunjucks/templating.html

### TS specific Nunjucks Filters
- `route(routeName: String)`
  ```
  {{ post | route('posts') }}
  ```
  Returns a relative path to a piece of content as defined by the routes in `tsg.yml`. The input Object must have the necessary fields specified in the route path in order to construct the path properly.  
- `md`
  ```
  {{ markdown | md }}
  ```
  Markdown to safe HTML using the CommonMark spec http://commonmark.org/   
- `numberFormat(format: String)`
  ```
  {{ numberField | numberformat(',.2r') }}
  # grouped thousands with two significant digits, 4200 -> "4,200"
  ```
  Returns a number formatted according to the the format specifier string https://github.com/d3/d3-format
- `code(language: String)`
  ```
  {{ codeField | code('javascript') }}
  ```
  Uses [prism.js](http://prismjs.com/) to return an HTML representation of the highlighted code. Takes an optional [language string](http://prismjs.com/#languages-list). You will need to manually include the corresponding [CSS](https://github.com/PrismJS/prism/tree/gh-pages/themes) in your project. 
- `image(params: Object)`
  ```
  {{ imageField | image({w: 320, h: 240, q: 90, crop: 'faces'}) }}
  ```
  Returns an imgix ready url. Takes an object of keys and values for any imgix filter https://docs.imgix.com/apis/url
- `date(format: String|Object)`
  ```
  {{ date | date('MMM Do YYYY') }}
  {{ date | date({format: 'MMM Do YYYY', tz: 'America/Los_Angeles') }}
  {{ date | date({format: 'LLL', tz: 'America/Los_Angeles', locale: 'fr') }}
  ```
  Formats dates using [moment.js](https://momentjs.com/). `format` can be either a [format string](https://momentjs.com/docs/#/displaying/format/) or an object where you can specify a format and override the default timezone and locale (configured in `tsg.yml`).


## Reach out
If we can make your life easier we want to hear from you at [support@takeshape.io](mailto:support@takeshape.io)
