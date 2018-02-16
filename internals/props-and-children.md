## Getting props and children of current component

Because you just pass arbitrary argument to reagent functions, you typically don't need to think about "props" and "children" as distinct things. But reagent does make a distinction and it is helpful to understand this particularly when interoperating with native elements and react libraries.

#### What are props in reagent?

Specifically, if the first argument to your reagent function is a map, that is assigned to \`this.props\` of the underlying reagent component. All other arguments are assigned as children to \`this.props.children\`.

```
   [my-div "one" "two"]                         ;; No props, two children
   [my-div {:first 1 :second 2} "one" "two"]    ;; Two props, two children
   [my-div "one" {:first 1 :second 2}  "two"]]) ;; Three children
```

#### Accessing props and children in a react-friendly way

When interacting with native react components, it may be helpful to access props and children, which you can do with \`reagent.core/current-component\`. This function returns an object that allows you retrieve the props and children passed to the current component.

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



