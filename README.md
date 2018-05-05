# React Project Coding Guideline
A coding guide line for React + Redux project @WORD UP

## Naming
#### File name & Folder name
  * Lowercase. Separated by single dash
    * `module/components/user-feedback.js`

#### Id & Class name
  * Lowercase. Separated by single dash
    * `nav-bar-item`

## File Structure
  * Use `module` to contains the files related to React & Redux
  * Creating folders is not required if it contains only one file.
    * `module/user-profile/`
      * `index.js` => might be a container
      * `components/`
        * `user-profile-component.js`
      * `reducers/`
        * `personal-info.js`
        * `subscription-histroy.js`
      * `personal-info-selector.js`
      * `actions/`
        * `generic-actions.js`
        * `personal-info.js`
        * `subscription-histroy.js`
      * `constants/`
        * `index.js`

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

#### All data in Store should be an immutable object created by `immutable.js` library

#### Data inside Store should be single source of truth
  * No duplicated data in different entries. If there are multiple entities need to refer to the same data, use foreign key like in a database.

#### Try to keep data doesn't have more than 2 nested levels
  * It's hard and error-prone to get/update data in a nested object. In addition, update data with a deep level will make several parent nodes being updated which may results in unnecessary render.
  * Use [normalizr](https://github.com/paularmstrong/normalizr) if needed.
  * [Normalizing State Shape](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape)

## General
#### Prefer `async/await` than `Promise.then().error`
  * The line of codes will be significantly lesser

#### Try as best as possible to avoid duplicated codes

#### Try as best as possible to avoid duplicated responsibilities
  * Duplicated codes are easy to be identified. Though some codes are not the same, but actually doing similar tasks. In that case, think if they should be combined into a component or a module class.

#### Be careful of injecting global events/variables
  * like `document.addEventlistener`.
    * Since we're in a SPA, remember to remove the listener at a proper timing like `componentDidUnmount`.
    * Take the parent/child tree into account. Don't add the same listener at both parents and children.
