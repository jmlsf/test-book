# React Interop Functions

### Creating React Elements directly

The `reagent.core/create-element` function simply calls react's `createElement` function \(and therefore, it expects either a string representing an HTML element or a react Component\).  

As an example, here are four ways to create the same element:

```
(defn integration []
  [:div
   [:div.foo "Hello " [:strong "world"]]

   (r/create-element "div"
                     #js{:className "foo"}
                     "Hello "
                     (r/create-element "strong"
                                        #js{}
                                        "world"))

   (r/create-element "div"
                     #js{:className "foo"}
                     "Hello "
                     (r/as-element [:strong "world"]))

   [:div.foo "Hello " (r/create-element "strong"
                                        #js{}
                                        "world")]])
```

### Creating React Elements from Hiccup forms

The `reagent.core/as-element` function creates a react element from a Hiccup form.  In the previous section, we discussed how `reagent/render` expects either \(1\) a Hiccup form or \(2\) a React Element.  If it encounters a Hiccup form, it calls `as-element` on it.  When you have a React component that wraps children, you can pass Hiccup forms to it wrapped in `as-element`.

### Creating Reagent "Components" from React Components

The function `reagent/adapt-react-class` will turn a React Component into something that can be placed into the first position of a Hiccup form, as if it were a Reagent function.  Take, for example the react-flip-move library and assume that it has been properly imported as a react Component called `FlipMove.`

```
[(reagent/adapt-react-class FlipMove)
  {:duration 500
   :easing "ease"}
  child-elements]
```





