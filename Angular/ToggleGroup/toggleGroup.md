# Handling multiple views

### Use Cases:
* Multiple views on the same page that we have to toggle between using a variable
* Group of tabs, activate the tab a user clicks on and hide the rest.

### Typical approaches to handling multiple views/tabs
* Single enum to keep track of the current tab
* Array of keys to keep track of the expanded or collapsed sections.
* Map operator to keep track of the sections states.

### What if we need to expand/collapse on multiple components?
* Here we can use **ToggleGroup**
    * Class capable of handling toggle functionality.
    * Since its a class, we can instantiate it and utilize its object to handle multiple views.

## ToggleGroup
* A class
* Collection of toggles or switches
* Allows you to switch on or off one or more keys.
* Allows you to expand or collapse all at once.
* Ability to get the status of a single or get all keys toggled on or off.
* Based on this status, we could easily manage our views.
* Composed of Toggle elements.
    * You can get the toggle control itself - key-value pair.

#### Installation:
* npm unstall toggle-group --save

#### Initialization:
```
// app.component.ts

import {ToggleGroup} from 'toggle-group';

class AppComponent implements OnInit {
    toggleGroup = new ToggleGroup(['Home', 'Posts', 'Chat']); // We keep this public since it will be used on our template

    ngOnInit(): void {
        this.toggleGroup.dropOpen('HOME');
    }
}

```

#### Handle Tabs:
**Open a tab**
* dropOpen(key: string): ToggleGroup

**Get status of a tab**
* getValue(key: string): ToggleGroup -> used to determine the status of a tab.

```
class AppComponent implements OnInit {
    tabTitles = ["Home", "Posts", "Job"];

    tg = new ToggleGroup(this.tabTitles);

    ngOnInit(): void {
        this.tg.dropOpen("Home");
    }
}

```
