# Atoms and cursors

Intro to atoms

Any component that dereferences a state atom will update whenever any part of it is updated.  If you are storing all state in a single atom \(not uncommon\), it will cause every component to update whenever the state is updated.  Performance-wise, this may be okay, depending on how many elements you have and how often your state updates, because react itself will not manipulate the DOM unless the components actually change.  But reagent provides two mechanisms to limit the degree to which components are re-rendered.

### Cursors

The simplest mechanism to isolate re-renders is to create a cursor, which is also an atom, from a specific key from the larger atom state, as in the following example.

```
(ns example
  (:require [reagent.core :as r]))
(defonce person (r/atom {:name
                         {:first-name "John" :last-name "Smith"}}))

(defn input [prompt val]
  [:div
   prompt
   [:input {:value @val
            :on-change #(reset! val (.-target.value %))}]])
                                                                           
(defn cursor-name-edit [n]
  (let [{:keys [first-name last-name]} @n]
    [:div
     [:p "I'm editing " first-name " " last-name "."]

     [input "First name: " (r/cursor n [:first-name])]
     [input "Last name:  " (r/cursor n [:last-name])]]))

(defn cursor-parent []
  [:div
   [:p "Current state: " (pr-str @person)]
   [cursor-name-edit (r/cursor person [:name])]])
```

In this example, changes to one of the `input`components will not affect the other, because each is tracking a different key in the primary state atom.  The `cursor-name-edit` and `cursor-parent` components will update when either changes.

* The cursor mechanism is more general than described above.  You can pass a function that performs arbitrary transformations on one or more atoms.  \(DO WE NEED TO EXPLAIN THIS?\)
* Reagent also provides the `reagent/wrap` mechanism, which also derives a new atom but provides more general functionality.  Where a cursor will always update the atom from which it was derived, `reagent/wrap` takes an atom and a callback that will be called whenever the derived atom is updated.  Replacing `(r/cursor n [:first-name])` with `(r/wrap first-name swap! n assoc :first-name)]` gives essentially the same results.  \(WHAT UTILITY DOES THIS HAVE?\)



