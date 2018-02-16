# current-component

## current-component

reagent.core/current-component returns an object that allows you retrieve the props and children passed to the current component.  

Beware that `current-component` is only valid in component functions, and must be called outside of e.g event handlers and for expressions, so it’s safest to always put the call at the top, as in `my-div` here:

```clojure
(ns example
  (:require [reagent.core :as r]))
(defn my-div []
  (let [this (r/current-component)]
    (into [:div.custom (r/props this)]
          (r/children this))))

(defn call-my-div []
  [:div
   [my-div "Some text."]
   [my-div {:style {:font-weight 'bold}}
    [:p "Some other text in bold."]]])
```



# Defining Methods

Methods allow you to smoothly display code examples in different languages.

{% method %}
## My first method

My first method exposes how to print a message in JavaScript and Go.

{% sample lang="js" %}
Here is how to print a message to `stdout` using JavaScript.

```js
console.log('My first method');
```

{% sample lang="go" %}
Here is how to print a message to `stdout` using Go.

```go
fmt.Println("My first method")
```

{% common %}
Whatever language you are using, the result will be the same.

```bash
$ My first method
```
{% endmethod %}
