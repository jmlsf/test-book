Here is an example of how easy it is to add undo functionality to Reagent components.

It simply saves the old state whenever it changes, and restores it when the button is clicked.

The really nice thing about ClojureScript is that not only is this easy and safe to do, courtesy of immutable data structures, it is also efficient. ClojureScript figures out how to represent ”changes” to maps and vectors efficiently, so that you won’t have to.

```clojure
(ns example
  (:require [reagent.core :as r]))
(def state todomvc/todos)

(def undo-list (r/atom nil))

(defn undo []
  (let [undos @undo-list]
    (when-let [old (first undos)]
      (reset! state old)
      (reset! undo-list (rest undos)))))


(defn undo-button []
  (let [n (count @undo-list)]
    [:input {:type "button" :on-click undo
             :disabled (zero? n)
             :value (str "Undo (" n ")")}]))

(defn todomvc-with-undo []
  (add-watch state ::undo-watcher
             (fn [_ _ old-state _]
               (swap! undo-list conj old-state)))
  [:div
   [undo-button]
   [todomvc/todo-app]])
```



