## Overview
What the user types in the search bar will be saved to list below (Search History).<br/>
Three files above are identical in execution, only difference is the approach.<br/>
You can delete items by clicking on it, items will **not** be saved upon reload.

### Approach1 
```html
<div id="app" ...>
    <todo v-on:additem="addItem"></todo>
    ...
</div>
```
```js
    Vue.component('todo', {
        ...
        methods: {
            pressEnter($event){
                this.$emit('additem',$event.target.value);
                $event.target.value = '';
            }
        }
    })
    new Vue({
        ...
        methods:{
            addItem(value){
                this.todos.push(value)
            },
            ...
        }
    })
```
