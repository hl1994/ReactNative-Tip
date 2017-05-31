## Eslint
npm i --save-dev eslint eslint-config-rallycoding

#### Tạo file .eslintrc
```js
{
    "parser": "babel-eslint",
    "env": {
        "node": true,
        "browser": true
    },
    "plugins": [
        "react"
    ],
    "extends": [
        "rallycoding",
        "plugin:react/recommended"
    ],
    "rules": {
        "quotes": ["off"]
    }
}
```
