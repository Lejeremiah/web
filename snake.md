#### 思维导图

![snake](C:\Users\27969\Desktop\snake.png)



#### js代码

```js
var sw = 20,
	sh = 20,
	td = 30,
	tr = 30;
var snake = null,
	food = null,
	game = null;

function square(x,y,classname){
	this.x = x*sw;
	this.y = y*sh;
	this.class = classname;

	this.viewContent = document.createElement("div");
	this.viewContent.className = this.class;
	this.parent = document.getElementById('snakeWrap');
}
square.prototype.create = function () {
	this.viewContent.style.width = sw + 'px';
	this.viewContent.style.height = sh + 'px';
	this.viewContent.style.position = 'absolute';
	this.viewContent.style.top = this.y + 'px';
	this.viewContent.style.left = this.x + 'px';
	this.parent.appendChild(this.viewContent);
}
square.prototype.remove = function (){
	this.parent.removeChild(this.viewContent);
}


function Snake() {
	this.head = null;
	this.tail = null;
	this.pos = [];

	this.directionNum = {
		right:{
			x:1,
			y:0,
			rotate:0
		},
		left:{
			x:-1,
			y:0,
			rotate:180
		},
		up:{
			x:0,
			y:-1,
			rotate:-90
		},
		down:{
			x:0,
			y:1,
			rotate:90
		}
	}
	this.direction = null;
}
Snake.prototype.init = function (){
	var snakeHead = new square(2,0,'snakeHead');
	snakeHead.create();
	this.head = snakeHead;
	this.pos.push([2,0]);

	var snakeBody1 = new square(1,0,'snakeBody');
	snakeBody1.create();
	this.pos.push([1,0]);

	var snakeBody2 = new square(0,0,'snakeBody');
	snakeBody2.create();
	this.pos.push([0,0]);
	this.tail = snakeBody2;

	snakeHead.last = null;
	snakeHead.next = snakeBody1;
	snakeBody1.last = snakeHead;
	snakeBody1.next = snakeBody2;
	snakeBody2.last = snakeBody1;
	snakeBody2.next = null;

	this.direction = this.directionNum.right;
}
Snake.prototype.getNextPos = function (){
	//下一个坐标
	var nextPos = [
		this.head.x/sw + this.direction.x,
		this.head.y/sh + this.direction.y
	]
	console.log(nextPos);
	//自身

	var selfCollies = false;
	this.pos.forEach(function (value){
		if(value[0] == nextPos[0] && value[1] == nextPos[1]){
			selfCollies = true;
		}
	})
	if(selfCollies){
		this.strategies.die.call(this);
		return ;
	}
	//撞墙
	if(nextPos[0] < 0 || nextPos[0] > 29 || nextPos[1] < 0 || nextPos[1] > 29){
		console.log('zhuangqiang le!');
		this.strategies.die.call(this);
		return ;
	}

	//食物
	// this.strategies.eat.call(this);
	// console.log(food);
	if(food && food.pos[0] == nextPos[0] && food.pos[1] == nextPos[1]){
		console.log('eat');
		this.strategies.eat.call(this);
		return ;
	}

	//前进
	console.log('move');
	this.strategies.move.call(this);
}

Snake.prototype.strategies = {
	move:function(format){
		var newBody = new square(this.head.x/sw, this.head.y/sh, 'snakeBody');
		newBody.last = null;
		newBody.next = this.head.next;
		newBody.next.last = newBody;

		this.head.remove();
		newBody.create();


		var newHead = new square(this.head.x/sw + this.direction.x, this.head.y/sh + this.direction.y, 'snakeHead');
		newHead.last = null;
		newHead.next = newBody;
		newBody.last = newHead;
		newHead.viewContent.style.transform='rotate('+this.direction.rotate+'deg)';
		newHead.create();

		this.head = newHead;
		this.pos.splice(0, 0, [this.head.x/sw, this.head.y/sh]);
		if(!format){
			this.tail = this.tail.last;
			this.tail.next.remove();
			this.pos.pop();
			game.score++;
		}

	}, 
	eat:function(){
		this.strategies.move.call(this, true);
		food.remove();
		createFood();
		// game.score++;
	},
	die:function(){
		console.log('die');
		game.over();
	}
}

function createFood(){
	var x = null;
	var y = null;

	var include = true;
	while(include){
		x = Math.round(Math.random()*(td - 1));
		y = Math.round(Math.random()*(tr - 1));

		snake.pos.forEach(function(value){
			if(x!=value[0] && y!=value[1]){
				include = false;
			}
		})
	}

	food = new square(x, y, 'food');
	food.pos = [x,y];
	food.create();
}
function Game(){
	this.timer = null;
	this.score = 0;
}
Game.prototype.init = function (){
	snake.init();
	createFood();

	document.onkeydown=function (ev){
		console.log(ev.which);
		if(ev.which == 37 || ev.which == 65 && snake.direction != snake.directionNum.right){
			snake.direction = snake.directionNum.left;
		}else if(ev.which == 38 || ev.which == 87 && snake.direction != snake.directionNum.down){
			snake.direction = snake.directionNum.up;
		}else if(ev.which == 39 || ev.which == 68 && snake.direction != snake.directionNum.left){
			snake.direction = snake.directionNum.right;
		}else if(ev.which == 40 || ev.which == 83 && snake.direction != snake.directionNum.up){
			snake.direction = snake.directionNum.down;
		}
		if(ev.which == 81){
		snake.strategies.move.call(snake, true);
		}
	}

	this.start();
}
Game.prototype.start = function (){
	this.timer = setInterval(function (){
		snake.getNextPos();
	},200);

}
Game.prototype.over = function (){
	clearInterval(this.timer);
	alert('得分为：' + this.score);

	var snakeWrap = document.getElementById('snakeWrap');
	snakeWrap.innerHTML = '';
	snake = new Snake();
	game = new Game();

	var startBtnWrap = document.querySelector('.startBtn');
	startBtnWrap.style.display = 'block';

}
Game.prototype.pause = function(){
	clearInterval(this.timer);
}



var snake = new Snake();
var game = new Game();

var startBtn = document.querySelector('.startBtn button');
startBtn.onclick = function (){
	startBtn.parentNode.style.display = 'none';
	game.init();
}
var snakeWrap=document.getElementById('snakeWrap');
var pauseBtn=document.querySelector('.pauseBtn button');
snakeWrap.onclick=function(){
	game.pause();
	pauseBtn.parentNode.style.display='block';
}
pauseBtn.onclick=function(){
	game.start();
	pauseBtn.parentNode.style.display='none';
}

```

#### 总结

1.this和call这个地方挺容易出错

2.square()传classname参数 表现不同的snake身体类型比较优

3.通过direcionNum改变位置

4.知识点查漏：call和this  foreEach循环  push splice pop对数组的操作  css3的rotate旋转蛇头



<img src="C:\Users\27969\Desktop\6fa1cfb3a52279bd937f238fd356298.png" alt="6fa1cfb3a52279bd937f238fd356298" style="zoom:38%;" />