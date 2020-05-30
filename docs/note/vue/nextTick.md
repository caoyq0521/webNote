# nextTick

通过`window.MutationObserver对象`实现简单的nextTick

````javascript
function nextTick(callback,ctx){
    let counter = 0
    let textNode = document.createTextNode(String(counter));
    let fn = ctx ? callback.bind(ctx) : callback;

    let watcher = new window.MutationObserver(fn);
    let options ={
            characterData : true
    }
    watcher.observe(textNode,options)

    counter++;
    textNode.data = String(counter)
};

// 测试
let name = 'dufu'
let person = {
    name : 'libai',
    show(){
        console.log(this.name)
    }
};

nextTick(person.show,person);
console.log('1');  
````