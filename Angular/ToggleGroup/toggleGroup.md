# Handling multiple views
Link - https://github.com/ar124official2019/toggle-group-dev#readme

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
```
dropOpen(key: string): ToggleGroup
```

**Get status of a tab**
```
getValue(key: string): ToggleGroup -> used to determine the status of a tab.
```

```
class AppComponent implements OnInit {
    tabTitles = ["Home", "Posts", "Job"];

    tg = new ToggleGroup(this.tabTitles);

    ngOnInit(): void {
        this.tg.dropOpen("Home");
    }
}

```

```
<div class="app">
    <div class="nav flex">
        <button *ngFor="let tab of tabTitles" (click)="tg.dropOpen(tab)>{{tab}}</button>
    </div>

    <div class="tab-view" *ngIf="tg.getValue('Home')">
        <h2>HOME</h2>
    </div>

    <div class="tab-view" *ngIf="tg.getValue('Posts')">
        <h2>Posts</h2>
    </div>

    <div class="tab-view" *ngIf="tg.getValue('Job')">
        <h2>Job</h2>
    </div>
</div>
```

### Other methods for expanding/collapsing
**Toggle:**
* Method toggles value on-off based on current value, against a key
```
toggle(key: string): ToggleGroup
```

**Turn On / View in:**
* Method open is used to toggle on a control, a key is passed to it, and it sets the value to true.
```
open(key: string): ToggleGroup
```

**Turn Off / View out:**
```
close(key: string): ToggleGroup
```

**Turn Off all controls:**
```
closeAll(): ToggleGroup
```

**Turn on all controls:**
```
openAll(): ToggleGroup
```

**Get value of a particular toggle**
```
getValue(key: string): boolean
```

**Set value of a particular toggle**
```
setValue(key: string, value: boolean)
```

**Together**
```
export class AppComponent {
    section = new ToggleGroup([
        {key: 'One', value: true},
        {key: 'Two', value: true},
        {key: 'Three', value: false},
        {key: 'Four', value: true},
        {key: 'Five', value: true},
    ]);
}
```


