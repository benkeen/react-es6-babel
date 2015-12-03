### react-es6-babel

- React 14
- ES6 & Babel
- React Router
- Redux

This is just a personal repo to make sense of all these new technologies that I'm ashamed to be behind on. 

Info pulled from:
- https://www.twilio.com/blog/2015/08/setting-up-react-for-es6-with-webpack-and-babel-2.html
- http://www.jonathan-petitcolas.com/2015/05/15/howto-setup-webpack-on-es6-react-application-with-sass.html

Absurdly long installation steps to just get a Hello World going. Oof. Will need to package these for easy use later. 
- `npm install --save react`
- `sudo npm install --save-dev webpack`
- `sudo npm install webpack-dev-server`
- `npm install --save-dev babel-loader`
- `npm install --save-dev babel-core`
- `npm install --save-dev babel-preset-es2015`
- `npm install --save-dev babel-preset-react`


React have split React and React.DOM, which makes sense since it's now being used in non-browser settings like native.

### CSS / SASS

Seems like webpack automatically packs all CSS into the same JS file too (? weird). This shows a technique on how to 
remove it to allow normal browser caching: 
http://www.jonathan-petitcolas.com/2015/05/15/howto-setup-webpack-on-es6-react-application-with-sass.html

Details from webpack here on how to split it out:
http://webpack.github.io/docs/stylesheets.html#separate-css-bundle


### Code Splitting, Multiple Entry Points

This is excellent. This allows bundles created for runtime loading, like what I hacked together with Grunt at CBC. 

Multiple Entry points are different: https://webpack.github.io/docs/multiple-entry-points.html - they're for generating 
multiple bundles loaded by the page (or wherever), e.g. a "libs.js" that loads all libraries, and an "app.js" for 
loading your app code. 


### Cache invalidation for bundled files

Just include a [hash] in the generated file.
https://webpack.github.io/docs/long-term-caching.html

But we need to use an additional plugin to access the unique asset filename that was created:
https://github.com/ampedandwired/html-webpack-plugin

###  Redux

This is a big piece of the puzzle. 

```
npm install --save redux
npm install --save react-redux
npm install --save-dev redux-devtools
```

#### Actions 

All very familiar pub-sub like stuff.

```javascript
var actionObject = { type: "action name", arbitraryData: 123 }; 
store.dispatch(actionObject);
```

#### Action Creators

Like typical Flux actions.js files that publish the actions, except they RETURN the actions.  


#### Reducers

Think about the state (store, I guess) shape. Quotes from the doc (http://rackt.org/redux/docs/basics/Reducers.html):

- try to keep the data separate from the UI state
- we suggest that you keep your state as normalized as possible, without any nesting
- think of the app’s state as a database

Never include these in a reducer:
- Mutate its arguments
- Perform side effects like API calls and routing transitions;
- Calling non-pure functions, e.g. Date.now() or Math.random().

"**Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API 
calls. No mutations. Just a calculation.***"

e.g.
```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

Note the Object.assign method passing an anon object which acts as the object to be modified - not the state obj.

- "reducer composition": the fundamental pattern of building Redux apps. Basically this is just sensible coding: passing
appropriate subsets of the stores to separate methods to properly return the updated state for that subset of the whole
state object.

##### combineReducers

Neat. 

Nice ES6 trick.

```javascript
import { combineReducers } from 'redux'
import * as reducers from './reducers'

const todoApp = combineReducers(reducers)
```


#### Store (singular!)




