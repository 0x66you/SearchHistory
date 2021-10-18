## Overview
What the user types in the search bar will be saved to list below (Search History).<br/>
Three files above are identical in execution, only difference is the approach.<br/>
You can delete items by clicking on it, items will **not** be saved upon reload.

### Approach 1
```html
<div id="app" ...>
    <todo v-on:additem="addItem"></todo>
    ...
</div>
```
```js
<script>
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
</script>
```
### Approach 2 
**same searches wont register twice** (see code snippet below)
```html
<div id="app" ...>
    <todo v-model="out"></todo>
    ...
</div>
```
```js
<script>
    Vue.component('todo',{
        ...
        methods:{
            pressEnter($event){
                this.$emit('input',$event.target.value);
                $event.target.value = '';
            }
        }
    })
    new Vue({
        el:'#app',
        data:{
            out:'',
            todos:[]
        },
        watch:{
            out(){
                if (this.out != ''){
                    this.todos.push(this.out);
                }
            }
        },
       ...
    })
</script>
```

### Approach 3
**no attributes needed in &#60;todo&#62; tag**
```html
<div id="app" class="container mt-3">
    <todo></todo>
    <ul class="list-group list-group-flush" v-for="todo in todos">
        <li class="list-group-item" @click="erase(todo)">{{ todo }}</li>
    </ul>
</div>
```
```js
<script>
    const eventBus = new Vue()
    Vue.component('todo',{
        ...
        methods:{
            pressEnter($event){
                eventBus.$emit('additem',$event.target.value);
                $event.target.value = '';
            }
        }
    })
    new Vue({
        el:'#app',
        data:{
            out:'',
            todos:[]
        },
        created(){
            eventBus.$on("additem",value => {
                this.todos.push(value)
            })
        },
       ...
    })
</script>
```


