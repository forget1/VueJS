自定义filter可以写在全局的Vue下，代码示例如下：

```javascript
Vue.filter('reverse', function(value) {
    return value.split('').reverse().join('');
})
```

也可以写在实例当中，代码示例如下：

```javascript
var demo = new Vue({
    el: '#demo',
    data: {},
    filters: {
        // 自定义filter事件的位置
        reverse: function(value) {
            return value.split('').reverse().join('');
        }
    },
    methods: {}
})
```

二者本质上无区别，可选择一种使用。但是采用Vue.filter时，需要在实例化Vue对象前定义，否则自定义的filter将不起作用。