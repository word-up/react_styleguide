# React Project Coding Guideline
An opinioned coding guideline for React + Redux project @WORD UP

## Naming
#### File name & Folder name
  * PascalCase for component(equal to component name)
    * `components/UserFeedback.js`
  * camelCase for helper function
    * `utils/dataFormater.js`

#### Id & Class name
  * Lowercase. Separated by single dash
    * `nav-bar-item`
  * However, `__` & `--` is still fine if using [BEM](http://getbem.com/) style.
    * eg: `.block__elem--mod`

## File Structure
```
  |- pages/           -> direct handler for each route
    |- account/
      |- __test__/
      |- index.js
      |- styles.scss
  |- containers/      -> connect to store
    |- PersonalInfo/
      |- __test__/
      |- index.js
      |- styles.scss
  |- components/  
    |- PersonalInfo/
      |- __test__/
      |- index.js
      |- styles.scss
  |- reducers/
    |- user/
      |- __test__/
      |- actions.js
      |- constants.js
      |- reducer.js
      |- selector.js
  |- styles/          -> common css like variable, mixin..
  |- utils/           -> helper function
```
## Spacing and Code Indentation
  * Use 2 spaces for indentation instead of `tab`

## Code Comments and Inline Documentation
  * Use backslash `// comments when necessary`
  * For todos use the format `//TODO: (with the person responsible and reason)`
    * for example: `//TODO: (From Mark) hookup Ajax to rest server`
  
## React
#### Use multiple lines when we need to pass more than one props.
  * Organize props in alphabetic order.
    ```javascript
    <Component
      aProp: 'a data'
      bProp: 'b data'
    />
    ```

#### Group `import` into sections
  * In order to understand where's the code comes from, group all of `import` statements logically
    ```javascript
    // group 1: import javascript native or 3rd libraries
    import React;
    import moment;

    // group 2: import components
    import Loader from 'Components/generic-tools/loader';
    import ProductList from 'Components/product-list';

    // group 3: import modules like utilities, actions, reducers ...
    import { fetchCourseById } from 'Utils/APIs/admin';
    import { toRoute } from 'Utils/route-handler';

    // group 4: import css, styles, class
    import classnames from 'classnames/bind';
    import styles from './styles.scss';
    ```
#### props
  * should prevent using too many props in single component
  * use proper naming ([reference](https://dlinau.wordpress.com/2016/02/22/how-to-name-props-for-react-components/))
    ```
    Array – use plural nouns. e.g. items
    Number – use prefix num or postfix count, index etc that can imply a number. e.g. numItems, itemCount, itemIndex
    Bool – use prefix is, can, has
      is: for visual/behavior variations. e.g. isVisible, isEnable, isActive
      can: fore behavior variations or conditional visual variations. e.g. canToggle, canExpand, canHaveCancelButton
      has: for toggling UI elements. e.g. hasCancelButton, hasHeader
    Object – use noun. e.g. item
    Node – use prefix node. containerNode
    Element – use prefix element. hoverElement
    ```


## Redux
#### Try to minimize the size of a reducer
  * A giant reducer is hard to understand and also usually implies it has too many responsibilities. Having a smaller reducer also helps to make its corresponding actions smaller and more focued.
  * We can use redux built in `#combineReducers` or [redux-immutable](https://github.com/gajus/redux-immutable) package
  * [Refactoring Reducers Example](https://redux.js.org/recipes/structuring-reducers/refactoring-reducers-example)
  * [Using combineReducers](https://redux.js.org/recipes/structuring-reducers/using-combinereducers)

#### Container should not have views inside unless it's very simple.
  * A container prepares data from store and dispatch actions and connected them to its component.

#### Use selectors to fetch specific data from Store.
  * `user-info-selector.js` may provides `#get_full_name`, `#get_last_order` methods which talk to Store.
  * Can also provides some reusable mutation functions.
    * [An example of using reusable state-mutation functions](https://tech.affirm.com/redux-patterns-and-anti-patterns-7d80ef3d53bc):
      ```javascript
      // utils.js
      const applyFn = (state, fn) => fn(state)
      export const pipe = (fns, state) => state.withMutations(s => fns.reduce(applyFn, s))

      // reducer.js
      return pipe([
        mutate.closeModal,
        mutate.stopLoading,
        mutate.updateLoan(action.loan),
      ], state)
      ```

#### All data in Store should be an immutable object created by `immutable.js` library

#### Data inside Store should be single source of truth
  * No duplicated data in different entries. If there are multiple entities need to refer to the same data, use foreign key like in a database.

#### Try to keep data doesn't have more than 2 nested levels
  * It's hard and error-prone to get/update data in a nested object. In addition, update data with a deep level will make several parent nodes being updated which may results in unnecessary render.
  * Use [normalizr](https://github.com/paularmstrong/normalizr) if needed.
  * [Normalizing State Shape](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape)

#### Combine `set` actions
  * Instead of using `state.set('key1', value1).set('key2', 'value2')`, use `withMutations` to wrap multiple actions into one update. Since Immutable.js will perform re-arranging in all the processes and retain all intermediate states.
  * eg: `state.withMutations(s => s.set('key1', value1).set('key2', 'value2'))`

#### Refrain from using `.toJS()`
  * It's a resource demanding action and also we lose the performance benefit. In addition, every `toJS()` call results in a new object which will always trigger unnecessary render.

## General
#### Prefer single quote or backtick for String.
  * `'a string'`
  * `` `a string with ${variable}` ``

#### Prefer `async/await` than `Promise.then().error`
  * The line of codes will be significantly lesser

#### Try as best as possible to avoid duplicated codes

#### Try as best as possible to avoid duplicated responsibilities
  * Duplicated codes are easy to be identified. Though some codes are not the same, but actually doing similar tasks. In that case, think if they should be combined into a component or a module class.

#### Be careful of injecting global events/variables
  * like `document.addEventlistener`.
    * Since we're in a SPA, remember to remove the listener at a proper timing like `componentDidUnmount`.
    * Take the parent/child tree into account. Don't add the same listener at both parents and children.

## eslint
apply `eslint-config-airbnb` with plugin `react` and `prettier`

## References
  * https://redux.js.org/
  * https://tech.affirm.com/redux-patterns-and-anti-patterns-7d80ef3d53bc
