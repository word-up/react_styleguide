# React Project Coding Guideline
An opinioned coding guideline for React + Redux project @WORD UP

## Naming
#### File name & Folder name
  * Lowercase. Separated by single dash
    * `module/components/user-feedback.js`

#### Id & Class name
  * Lowercase. Separated by single dash
    * `nav-bar-item`
  * However, `__` & `--` is still fine if using [BEM](http://getbem.com/) style.
    * eg: `.block__elem--mod`

## File Structure
  * Use `use-cases` to contains the files related to React & Redux
  * Reference:
    * [Tips For a Better Redux Architecture: Lessons for Enterprise Scale](https://hashnode.com/post/tips-for-a-better-redux-architecture-lessons-for-enterprise-scale-civrlqhuy0keqc6539boivk2f)
    * [Architecture the Lost Years by Robert Martin](https://www.youtube.com/watch?v=WpkDN78P884)
  * `pages/` -> direct handler for each route
    * `account/`
      * `index.js`
  * `use-cases/`
    * `user-profile/`
      * `containers/`
        * `personal-info-container.js`
        * `__test__/`
          * `personal-info-container.test.js`
      * `components/`
        * `user-profile-component.js`
        * `__test__/`
          * `user-profile-component.test.js`
      * `reducers/`
        * `personal-info.js`
        * `subscription-histroy.js`
        * `__test__/`
          * `personal-info.test.js`
          * `subscription-histroy.test.js`
      * `selectors/`
        * `personal-info-selector.js`
        * `subscription-histroy-selector.js`
        * `__test__/`
          * `personal-info-selector.test.js`
          * `subscription-histroy-selector.test.js`
      * `actions/`
        * `generic-actions.js`
        * `personal-info.js`
        * `subscription-histroy.js`
        * `__test__/`
          * `generic-actions.test.js`
          * `personal-info.test.js`
          * `subscription-histroy.test.js`
      * `constants/`
        * `index.js`
    * `product-list/`
      * `...`
## Spacing and Code Indentation
  * Use 2 spaces and indent using spaces

## Code Comments and Inline Documentation
  * Use backslash // comments when necessary
  * For todos use the format //TODO: with the person responsible and reason `//TODO: (From Mark) hookup Ajax to rest server`
  
## React
#### Add `propTypes` to every components
  * It helps others to use the component easily and also help to catch some errors quickly.
  * In alphabetic order

#### Use multiple lines when we need to pass more than one props.
  * Organize props in alphabetic order.
    ```javascript
    <Component
      aProp: 'a data'
      bProp: 'b data'
    />
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

## Under consideration (welcome ideas as well as debates)
  * Separate `sync` and `async` Redux actions into different files.

## References
  * https://redux.js.org/
  * https://tech.affirm.com/redux-patterns-and-anti-patterns-7d80ef3d53bc
