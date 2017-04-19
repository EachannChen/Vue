# Vue.js实现TODOList
首先来看一下，演示效果：
![这里写图片描述](http://img.blog.csdn.net/20170419211756849?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGFvZ2UxMDI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 页面布局
![这里写图片描述](http://img.blog.csdn.net/20170419212908370?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGFvZ2UxMDI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

模板文件：
```
<!DOCTYPE html>
<html lang="en"><head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="index.css">
</head>
<body>
    <div class="page-top">
        <div class="page-content">
            <h2>任务计划列表</h2>
        </div>
    </div>
    <div class="main">
    	<h3 class="big-title">添加任务：</h3> 
    	<input placeholder="例如：吃饭睡觉打豆豆；    提示：+回车即可添加任务" type="text" class="task-input"> 
    		<ul class="task-count">
    			<li>5个任务未完成</li> 
    			<li class="action">
    				<a href="#all" class="">所有任务</a> 
    				<a href="#unfinished" class="">未完成的任务</a> 
    				<a href="#finished" class="">完成的任务</a>
    			</li>
    		</ul> 
    		<h3 class="big-title">任务列表：</h3> 
    		<div class="tasks">
    			<span class="no-task-tip" style="display: none;">还没有添加任何任务</span>
    			<ul class="todo-list">
    				<li class="todo">
    					<div class="view">
    						<input type="checkbox" class="toggle">
    						<label>1</label> 
    						<button class="destroy"></button>
    					</div> 
    					<input type="text" class="edit">
    				</li>
    				<li class="todo">
    					<div class="view">
    						<input type="checkbox" class="toggle"> 
    						<label>2</label>
    						<button class="destroy"></button>
    					</div> 
    					<input type="text" class="edit">
    				</li>
    				<li class="todo">
    					<div class="view">
    						<input type="checkbox" class="toggle"> 
    						<label>3</label>
    						<button class="destroy"></button>
    					</div> 
    					<input type="text" class="edit">
    				</li>
    			</ul>
    		</div>
    </div>
</body>
</html>
```
css文件：

```
body {
    margin:0;
    background-color: #fafafa;
    font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

h2{
    margin:0;
    font-size: 12px;
}
a {
    color: #000;
    text-decoration: none;
}

input {
    outline: 0;
}

button {
    margin: 0;
    padding: 0;
    border: 0;
    background: none;
    font-size: 100%;
    vertical-align: baseline;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    outline: 0;
}

ul {
    padding:0;
    margin:0;
    list-style: none;
}

.page-top {
    width: 100%;
    height: 40px;
    background-color: #db4c3f;
}

.page-content {
    width: 50%;
    margin: 0px auto;
}

.page-content h2{
    line-height: 40px;
    font-size: 18px;
    color: #fff;
}

.main {
    width: 50%;
    margin: 0px auto;
    box-sizing:border-box;
}

.task-input {
    width: 99%;
    height:30px;
    outline: 0;
    border: 1px solid #ccc;
}

.task-count{
    display: flex;
    margin:10px 0;
}

.task-count li {
    padding-left: 10px;
    flex: 1;
    color: #dd4b39;
}

.task-count li:nth-child(1){
    padding: 5px 0 0 10px;
}

.action {
    text-align: center;
    display: flex;
}
.action a {
    margin: 0px 10px;
    flex: 1;
    padding: 5px 0;
    color: #777;

}

.action a:nth-child(3){
    margin-right: 0;
}

.active {
    border: 1px solid rgba(175, 47, 47, 0.2);
}

.tasks {
    background-color: #fff;
}
.no-task-tip {
    padding:10px 0 10px 10px;
    display: block;
    border-bottom: 1px solid #ededed;
    color:#777;
}

.big-title {
    color: #222;
}


.todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
}

.todo-list li {
    position: relative;
    font-size: 16px;
    border-bottom: 1px solid #ededed;
}

.todo-list li:hover {
    background-color: #fafafa;
}


.todo-list li.editing {
    border-bottom: none;
    padding: 0;
}

.todo-list li.editing .edit {
    display: block;
    width: 506px;
    padding: 13px 17px 12px 17px;
    margin: 0 0 0 43px;
}

.todo-list li.editing .view {
    display: none;
}

.todo-list li .toggle {
    text-align: center;
    width: 40px;
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none; 
    -webkit-appearance: none;
    appearance: none;
}

.toggle {
    text-align: center;
    width: 40px;
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none; 
    -webkit-appearance: none;
    appearance: none;
}

 .toggle:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
}

.toggle:checked:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}

.todo-list li label {
    white-space: pre-line;
    word-break: break-all;
    padding: 15px 60px 15px 15px;
    margin-left: 45px;
    display: block;
    line-height: 1.2;
    transition: color 0.4s;
}

.todo-list li.completed label {
    color: #d9d9d9;
    text-decoration: line-through;
}


.todo-list li .destroy {
    display: none;
    position: absolute;
    top: 0;
    right: 10px;
    bottom: 0;
    width: 40px;
    height: 40px;
    margin: auto 0;
    font-size: 30px;
    color: #cc9a9a;
    margin-bottom: 11px;
    transition: color 0.2s ease-out;
}

.todo-list li .destroy:hover {
    color: #af5b5e;
}

.todo-list li .destroy:after {
    content: '×';
}

.todo-list li:hover .destroy {
    display: block;
}

.todo-list li .edit {
    display: none;
}

.todo-list li.editing:last-child {
    margin-bottom: -1px;
}
```
## 功能实现
>1.引入vue.js文件

```
 <script src="./vue.js"></script>
```
>2.新建一个app.js并在最后面引入

```
<script src="./app.js"></script>
```
>3.在app.js中进行功能实现

```

//存取localStorage中的数据

var store = {
	save(key,value){
		localStorage.setItem(key,JSON.stringify(value));
	},
	fetch(key){
		return JSON.parse(localStorage.getItem(key)) || [];
	}
}



//取出所有的值
var list = store.fetch("mydata");

//过滤的时候有三种情况 all finished unfinished

var filter = {
	all:function(list){
		return list;
	},
	finished:function(list){
		return list.filter(function(item){
			return item.isChecked;
		})
	},
	unfinished:function(){
		return list.filter(function(item){
			return !item.isChecked;
		})
	}
}

var vm = new Vue({
	el:".main",
	data:{
		list:list,
		todo:"",
		edtorTodos:'',  //记录正在编辑的数据
		beforeTitle:'', //记录正在编辑的数据的title
		visibility: "all" //通过这个属性值的变化对数据进行筛选
	},
	watch:{
		list:{
			handler:function(){
				store.save("mydata",this.list);
			},
			deep:true
		}
	},
	computed:{
		noCheckeLength:function(){
			return this.list.filter(function(item){
                return !item.isChecked
            }).length
		},
		filteredList:function(){			
			//找到了过滤函数，就返回过滤后的数据；如果没有返回所有数据
			return filter[this.visibility] ? filter[this.visibility](list) : list;

		}
	},
	methods:{
		addTodo(){  //添加任务
			this.list.push({
				title:this.todo,
				isChecked:false
			});
			this.todo = '';

		},
		deleteTodo(todo){ //删除任务
			var index = this.list.indexOf(todo);
			this.list.splice(index,1);

		},
		edtorTodo(todo){  //编辑任务
			console.log(todo);
			//编辑任务的时候，记录一下编辑这条任务的title，方便在取消编辑的时候重新给之前的title
			this.beforeTitle = todo.title;

			this.edtorTodos = todo;



		},
		edtorTodoed(todo){ //编辑任务成功
			this.edtorTodos = '';
		},
		cancelTodo(todo){  //取消编辑任务

			todo.title = this.beforeTitle;

			this.beforeTitle = '';

			//让div显示出来，input隐藏
			this.edtorTodos = '';
		}
	},
	directives:{
		"foucs":{
			update(el,binding){
				if(binding.value){
					el.focus();
				}
			}
		}
	}
});

function watchHashChange(){
	var hash = window.location.hash.slice(1);

	vm.visibility = hash;
	
}

watchHashChange();

window.addEventListener("hashchange",watchHashChange);
```
源码已托管在Github上，如有疑问可在下方进行评论。
https://github.com/taoge1024/Vue/tree/master/demo/todoList



