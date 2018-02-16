# How Reagent Works

A little understanding of what reagent is doing really helps when you want to use the large set of react libraries.

The entry point to reagent is `reagent/render`.  All reagent programs have mount code that looks something like this:

```
(defn mount-root []
  (reagent/render [top-level-component]
    (.getElementById js/document "app")))
```

The key to understanding reagent is this: the render component takes one of two things:

1. A ClojureScript vector \(i.e., a Hiccup form\)
2. A React Element

If it encounters a ClojureScript vector, it will interpret it as a Hiccup and will either create a React Element directly \(if it is a native HTML element like `:div` or `:span`, or it will create \(and cache\) a React Component based on the function in the first element of the Hiccup form and then create a React Element from that Component.  CROSS REFERENCE HERE TO FORM1-2-3 DOC.

If it encounters a React Element, it doesn't need to do anything and will just pass that to react's render function.

This is why you can get away with calling simple reagent function as functions instead of returning them as vectors:

```
(defn simple-div []
  [:div "Hello"])

(defn mount-root []
  (reagent/render (simple-div)
    (.getElementById js/document "app")))
```

The invocation of `simple-div` will return a HIccup vector `[:div "Hello"]` , which the render function understand how to turn into a React Element.

