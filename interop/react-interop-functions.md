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

The function `reagent/adapt-react-class` will turn a React Component into something that can be placed into the first position of a Hiccup form, as if it were a Reagent function.  Take, for example the react-flip-move library and assume that it has been properly imported as a react Component called `FlipMove`.  By wrapping FlipMove with `adapt-react-class`, we can use it in a Hiccup form:

```
(defn top-article [articles]
  [(reagent/adapt-react-class FlipMove)
   {:duration 750
    :easing "ease-out"}
   articles]
```

There is also a convenience mechanism `:>` \(colon greater-than\) that shortens this and avoid some parenthesis:

```
(defn top-article [articles]
  [:> FlipMove
   {:duration 750
    :easing "ease-out"}
   articles]
```

This is the equivalent JavaScript:

```
const TopArticles = ({ articles }) => (
  <FlipMove duration={750} easing="ease-out">
    {articles}
  </FlipMove>
);
```

### Creating React Components from Reagent "Components"

The `reagent/reactify-component` will take a reagent "component" \(any the form-1, form-2, or form-3 reagent components\).  For example:

```
(defn exported [props]
  [:div "Hi, " (:name props)])

(def react-comp (r/reactify-component exported))

(defn could-be-jsx []
  (r/create-element react-comp #js{:name "world"}))
```

Note that `adapt-react-class` and `reactify-component` are not perfectly symmetrical, because `reactify-component` requires that the reagent component accept everything in a single props map, including its children.  \(Also note that `reagent/children` is helpful here in extracting the children passed to a reactified component.  SEE CROSS REFERENCE.\)

### Example: "Decorator" Higher-Order Components

Some react libraries use the decorator pattern: a react component which takes a component as an argument and returns a new component as its result.  One example is the React DnD library.  We will need to use both `adapt-react-class` and `reactify-component` to move back and forth between react and reagent:

```
(defn react-dnd-component
  []
  (let [decorator (DragDropContext HTML5Backend)]
    [(reagent/adapt-react-class
       (decorator (reagent/reactify-component top-level-component)))]))
```

This is the equivalent javascript:

```
import HTML5Backend from 'react-dnd-html5-backend';
import { DragDropContext } from 'react-dnd';

class TopLevelComponent {
  /* ... */
}

export default DragDropContext(HTML5Backend)(TopLevelComponent);
```



