# Overview
This is an app I created using create-react-app to demonstrate issues with the linaria babel plugin. 

The app gets its css from `/src/App.style.js`. That file utilizes a value in `src/config.json` inside 
a linaria tag in order to demonstrate the first issue I encountered, where json files were being evaluated
as if they were Javascript files. My solution to that was to add the `.json` extension to the regex of ignored
files in the `loadLinariaOptions` module of the linaria repo.

The second issue I encountered was the following error:

``` console
 linaria:transform:00001 [stage-1] >> (__linariaPreval) +1ms
  linaria:preeval:00001 [start] Looking for template literals… +0ms
  linaria:template-parse:identify-expressions 1 +0ms
SyntaxError: /Users/kcover/Projects/babel-playground-app/src/App.style.js: /Users/kcover/Projects/babel-playground-app/src/App.style.js: displayName.charAt is not a function
  2 | import config from './config.json';
  3 |
> 4 | export const StyledApp = styled.div`
    |                          ^^^^^^
  5 | .App {
  6 |   text-align: center;
  7 | }
    at File.buildCodeFrameError (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/core/lib/transformation/file/file.js:205:12)
    at NodePath.buildCodeFrameError (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/path/index.js:105:21)
    at buildCodeFrameError (/Users/kcover/Projects/linaria/packages/babel/lib/utils/getTagProcessor.js:30:17)
    at getTagProcessor (/Users/kcover/Projects/linaria/packages/babel/lib/utils/getTagProcessor.js:328:15)
    at processTemplateExpression (/Users/kcover/Projects/linaria/packages/babel/lib/utils/processTemplateExpression.js:13:53)
    at Identifier (/Users/kcover/Projects/linaria/packages/babel/lib/plugins/preeval.js:53:50)
    at NodePath._call (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/path/context.js:45:20)
    at NodePath.call (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/path/context.js:35:17)
    at NodePath.visit (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/path/context.js:80:31)
    at TraversalContext.visitQueue (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/context.js:86:16)
    at TraversalContext.visitSingle (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/context.js:65:19)
    at TraversalContext.visit (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/context.js:109:19)
    at traverseNode (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/traverse-node.js:18:17)
    at NodePath.visit (/Users/kcover/Projects/babel-playground-app/node_modules/@babel/traverse/lib/path/context.js:86:52)
```

If you've linked a version of linaria (with `npm` or `yarn link`) with the first issue fixed you can see the error by running
`DEBUG=linaria* LINARIA_LOG=debug yarn babel -o dist.js src/App.style.js`

NOTE: You can also uncomment the rule in `linaria.config.js` to fix the json issue.
