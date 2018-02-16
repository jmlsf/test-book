```
# Getting props and children of current component
```


    Because you just pass argument to reagent functions, you typically don't need to think about "props" and "children" as distinct things.  But reagent does make a distinction and it is helpful to understand this particularly when interoperating with native elements and react libraries.  

    Specifically, if the first argument to your reagent function is a map, that is assigned to `this.props` of the underlying reagent component.  All other arguments are assigned as children to `this.props.children`.

    When interacting with native react components, it may be helpful to access props and children, which you can do with `reagent.core/current-component`.  This function returns an object that allows you retrieve the props and children passed to the current component.  

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







