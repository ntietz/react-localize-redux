## Pass multiple translations to components

To avoid connecting every single component that needs translations you should instead pass translations down to child components.
To retrieve multiple translations using [translate](../api/selectors#translatekey-string-string-data) pass an array of translation keys instead of a single key. This will return an object with translated strings mapped to translation keys.

Since `translate` returns an object we can use the object spread operator to pass the translations as props for the `<Article>` component.

```javascript
const translationData = {
  heading: ["Heading", "Heading French"],
  article: {
    title: ["Title", "Title French"],
    author: ["By ${ name }", "By French ${ name }"],
    desc: ["Description", "Description French"]
  }
};

const Page = ({ translate }) => (
  <div>
    <h1>{ translate('heading') }</h1>
    <Article { ...translate(['article.title', 'article.author', 'article.desc'], { name: 'Ted' }) } />
  </div>
);

const Article = props => (
  <div>
    <h2>{ props['article.title'] }</h2>
    <h3>{ props['article.author'] }</h3>
    <p>{ props['article.desc'] }</p>
  </div>
);
```


---------------



## Insert dynamic content into translations

Insert dynamic content into your translation strings by inserting placeholders with the following format `${ placeholder }`.

```json
{
  "greeting": [
    "Hello ${ name }",
    "Bonjour ${ name }"
  ]
}
```

Then pass in the data you want to swap in for placeholders to the [translate](../api/selectors#translatekey-string-string-data) function.

```javascript
<h1>{ translate('greeting', { name: 'Testy McTest' }) }</h1>
```


---------------



### Include HTML in translations

Include HTML in your translation strings and it will be rendered in your component.

```json
{
  "google-link": [
    "<a href='https://www.google.en/'>Google</a>",
    "<a href='https://www.google.fr/'>Google</a>"
  ]
}
```



---------------



### Avoid naming collisions with nested translation data

** Multiple language format **

```json
{
  "welcome": {
    "greeting": [
      "Hello ${ name }!",
      "Bonjour ${ name }!"
    ]
  },
  "info": {
    "greeting": [
      "Hello",
      "Bonjour"
    ]
  }
}
```

** Single language format **

```json
{
  "welcome": {
    "greeting": "Hello ${ name }!"
  },
  "info": {
    "greeting": "Hello"
  }
}
```

```javascript
<h1>{ translate('welcome.greeting', { name: 'Testy McTest' }) }</h1>
<h1>{ translate('info.greeting') }</h1>
```


---------------


## Load translation data on demand

If you have a larger app you may want to break your translation data up into multiple files, or maybe your translation data is being loaded from a service. You can call [addTranslation](#addtranslationdata) for each new translation file/service, and the new translation data will be added to your Redux store.

Also If you are using a tool like [webpack](https://webpack.js.org) for bundling, then you can use [async code-splitting](https://webpack.js.org/guides/code-splitting-async/) to split translations across bundles, and async load them when you need them.