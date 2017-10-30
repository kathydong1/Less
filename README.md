### CSS简介

		CSS（层叠样式表）是一门历史悠久的标记性语言，HTML主要负责文档结构的定义，CSS负责文档表现形式或样式的定义。
		作为一门标记性语言，CSS的语法相对简单，对使用者的要求较低，但同时也带来一些问题：
		
			CSS需要书写大量看似没有逻辑的代码，不方便维护及扩展，不利于复用，尤其对于非前端开发工程师来讲，
			往往会因为缺少CSS编写经验而很难写出组织良好且易于维护的CSS代码，
			造成这些困难的很大原因源于CSS是一门非程序式语言，没有变量、函数、SCOPE（作用域）等概念。
			LESS为 Web开发者带来了福音，它在CSS的语法基础之上，引入了变量，Mixin（混入），运算以及函数等功能，
			大大简化了CSS的编写，并且降低了CSS的维护成本，
			就像它的名称所说的那样，LESS可以让我们用更少的代码做更多的事情。

### LESS原理及使用方式

		本质上，LESS 包含一套自定义的语法及一个解析器，用户根据这些语法定义自己的样式规则，
		这些规则最终会通过解析器，编译生成对应的CSS文件。LESS并没有裁剪CSS原有的特性，更不是用来取代CSS的，
		而是在现有CSS语法的基础上，为CSS加入程序式语言的特性。下面是一个简单的例子：
		
			1.LESS文件
			
				@color: #4D926F; 
				#header { 
					color: @color; 
				} 
				h2 { 
					color: @color; 
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
			#header { 
				color: #4D926F; 
			} 
			h2 { 
				color: #4D926F; 
			}

## 语法

### 变量

		LESS允许开发者自定义变量，变量可以在全局样式中使用，变量使得样式修改起来更加简单。
		我们可以从下面的代码了解变量的使用及作用：
		
			1.LESS文件
			
				@border-color : #b5bcc7; 
				.mythemes tableBorder{ 
					border : 1px solid @border-color; 
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				.mythemes tableBorder { 
					border: 1px solid #b5bcc7; 
				}
				
			从上面的代码中我们可以看出，变量是VALUE（值）级别的复用，可以将相同的值定义成变量统一管理起来。
			该特性适用于定义主题，我们可以将背景颜色、字体颜色、边框属性等常规样式进行统一定义，
			这样不同的主题只需要定义不同的变量文件就可以了
			
		LESS中的变量和其他编程语言一样，可以实现值的复用，同样它也有生命周期，
		也就是Scope（变量范围，开发人员惯称之为作用域），简单的讲就是局部变量还是全局变量的概念，
		查找变量的顺序是先在局部定义中找，如果找不到，则查找上级定义，直至全局。
		下面我们通过一个简单的例子来解释Scope：
		
			1.LESS文件
			
				@width : 20px; 
				#homeDiv { 
				  	@width : 30px; 
				  	#centerDiv{ 
				      	width : @width; //此处应该取最近定义的变量 width的值 30px
					}
				} 
				#leftDiv { 
				    width : @width; //此处应该取最上面定义的变量 width的值 20px
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				#homeDiv #centerDiv { 
					width: 30px; 
				} 
				#leftDiv { 
					width: 20px; 
				}

### Mixins（混入）

		Mixins（混入）功能对用开发者来说并不陌生，很多动态语言都支持 Mixins（混入）特性，它是多重继承的一种实现，
		在LESS中，混入是指在一个CLASS中引入另外一个已经定义的CLASS，就像在当前CLASS中增加一个属性一样。
		我们先简单看一下Mixins在LESS中的使用：
		
			1.LESS文件
			
				// 定义一个样式选择器
				.roundedCorners(@radius:5px) { 
					-moz-border-radius: @radius; 
					-webkit-border-radius: @radius; 
					border-radius: @radius; 
				} 
				// 在另外的样式选择器中使用
				#header { 
					.roundedCorners; 
				} 
				#footer { 
					.roundedCorners(10px); 
				}
			
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				#header { 
					-moz-border-radius:5px; 
					-webkit-border-radius:5px; 
					border-radius:5px; 
				} 
				#footer { 
					-moz-border-radius:10px; 
					-webkit-border-radius:10px; 
					border-radius:10px; 
				}
				
			从上面的代码我们可以看出：Mixins其实是一种嵌套，它允许将一个类嵌入到另外一个类中使用，
			被嵌入的类也可以称作变量，简单的讲，Mixins其实是规则级别的复用。
			
		Mixins还有一种形式叫做Parametric Mixins（混入参数），LESS也支持这一特性。
			
			1.LESS文件
			
				// 定义一个样式选择器
				.borderRadius(@radius){ 
					-moz-border-radius: @radius; 
					-webkit-border-radius: @radius; 
					border-radius: @radius; 
				} 
				// 使用已定义的样式选择器
				#header { 
					.borderRadius(10px); // 把 10px作为参数传递给样式选择器
				} 
				.btn { 
					.borderRadius(3px);// // 把 3px作为参数传递给样式选择器
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				#header { 
					-moz-border-radius: 10px; 
					-webkit-border-radius: 10px; 
					border-radius: 10px; 
				} 
				.btn { 
					-moz-border-radius: 3px; 
					-webkit-border-radius: 3px; 
					border-radius: 3px; 
				}
				
		我们还可以给Mixins的参数定义一个默认值
		
			1.LESS文件
			
				.borderRadius(@radius:5px){ 
					-moz-border-radius: @radius; 
					-webkit-border-radius: @radius; 
					border-radius: @radius; 
				} 
				.btn { 
					.borderRadius; 
				}
			
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				.btn { 
					-moz-border-radius: 5px; 
					-webkit-border-radius: 5px; 
					border-radius: 5px; 
				}
				
		像JavaScript中arguments一样，Mixins也有这样一个变量：@arguments。
		@arguments在Mixins中具是一个很特别的参数，当Mixins引用这个参数时，
		该参数表示所有的变量，很多情况下，这个参数可以省去你很多代码。
		
			1.LESS文件
			
				.boxShadow(@x:0,@y:0,@blur:1px,@color:#000){ 
					-moz-box-shadow: @arguments; 
					-webkit-box-shadow: @arguments; 
					box-shadow: @arguments; 
				} 
				#header { 
					.boxShadow(2px,2px,3px,#f36); 
				}
				
			经过编译生成的CSS文件如下：
				
			2.CSS文件
			
				#header { 
					-moz-box-shadow: 2px 2px 3px #FF36; 
					-webkit-box-shadow: 2px 2px 3px #FF36; 
					box-shadow: 2px 2px 3px #FF36; 
				}
				
		Mixins是LESS中很重要的特性之一，我们这里也写了很多例子，看到这些例子你是否会有这样的疑问：
		当我们拥有了大量选择器的时候，特别是团队协同开发时，如何保证选择器之间重名问题？
		LESS也采用了命名空间的方法来避免重名问题，于是乎LESS在mixins的基础上扩展了一下，看下面这样一段代码：
		
			1.LESS文件
			
				#mynamespace { 
					.home {...} 
					.user {...} 
				}
				
			这样我们就定义了一个名为mynamespace的命名空间，如果我们要复用user这个选择器的时候，
			我们只需要在需要混入这个选择器的地方这样使用就可以了：
				#mynamespace > .user

### 嵌套的规则

		在我们书写标准CSS的时候，遇到多层的元素嵌套这种情况时，我们要么采用从外到内的选择器嵌套定义，
		要么采用给特定元素加CLASS或ID的方式。在LESS中我们可以这样写：
		
			1.HTML片段
			
				<div id="home"> 
					<div id="top">top</div> 
					<div id="center"> 
						<div id="left">left</div> 
						<div id="right">right</div> 
					</div> 
				</div>
				
			2.LESS文件
			
				#home{ 
				  	color : blue; 
				  	width : 600px; 
				  	height : 500px; 
				  	border : outset; 
				  	#top{ 
				       	border : outset; 
				       	width : 90%; 
				  	}
				  	#center{ 
				       	border : outset; 
				       	height : 300px; 
				       	width : 90%; 
				       	#left{ 
				         	border : outset; 
				         	float : left; 
				 			width : 40%; 
				       	}
				       	#right{ 
				         	border : outset; 
				         	float : left; 
				 			width : 40%; 
				       	}
				   	}
				}
			
			经过编译生成的CSS文件如下：
			
			3.CSS文件
			
				#home { 
				 	color: blue; 
				 	width: 600px; 
				 	height: 500px; 
				 	border: outset; 
				} 
				#home #top { 
				 	border: outset; 
				 	width: 90%; 
				} 
				#home #center { 
				 	border: outset; 
				 	height: 300px; 
				 	width: 90%; 
				} 
				#home #center #left { 
				 	border: outset; 
				 	float: left; 
				 	width: 40%; 
				} 
				#home #center #right { 
				 	border: outset; 
				 	float: left; 
				 	width: 40%; 
				}
				
		从上面的代码中我们可以看出，LESS的嵌套规则的写法是HTML中的DOM结构相对应的，
		这样使我们的样式表书写更加简洁和更好的可读性。同时，嵌套规则使得对伪元素的操作更为方便。
		
			1.LESS文件
			
				a { 
				 	color: red; 
				 	text-decoration: none; 
				 	&:hover { // 有 & 时解析的是同一个元素或此元素的伪类，没有 & 解析是后代元素
					  	color: black; 
					  	text-decoration: underline; 
				 	}
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				a { 
					color: red; 
					text-decoration: none; 
				} 
				a:hover { 
					color: black; 
					text-decoration: underline; 
				}

### 运算及函数

		在我们的CSS中充斥着大量的数值型的value，比如color、padding、margin等，
		这些数值之间在某些情况下是有着一定关系的，那么我们怎样利用LESS来组织我们这些数值之间的关系呢？
		我们来看这段代码：
		
			1.LESS文件
			
				@init: #111111; 
				@transition: @init*2; 
				.switchColor { 
					color: @transition; 
				}
				
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				.switchColor { 
				 	color: #222222; 
				}
			
		上面的例子中使用LESS的operation是特性，其实简单的讲，
		就是对数值型的value（数字、颜色、变量等）进行加减乘除四则运算。
		同时LESS还有一个专门针对color的操作提供一组函数。
		下面是LESS提供的针对颜色操作的函数列表：
		
			lighten(@color, 10%); // return a color which is 10% *lighter* than @color 
			darken(@color, 10%); // return a color which is 10% *darker* than @color 
			saturate(@color, 10%); // return a color 10% *more* saturated than @color 
			desaturate(@color, 10%);// return a color 10% *less* saturated than @color 
			fadein(@color, 10%); // return a color 10% *less* transparent than @color 
			fadeout(@color, 10%); // return a color 10% *more* transparent than @color 
			spin(@color, 10); // return a color with a 10 degree larger in hue than @color 
			spin(@color, -10); // return a color with a 10 degree smaller hue than @color
			
		PS：上述代码引自LESS CSS官方网站，详情请见 http://lesscss.org/#-color-functions
		
		使用这些函数和JavaScript中使用函数一样。
		
			1.LESS文件
			
				init: #f04615; 
			 	#body { 
			  		background-color: fadein(@init, 10%); 
			 	}
			 	
			经过编译生成的CSS文件如下：
			
			2.CSS文件
			
				#body { 
				 	background-color: #f04615; 
				}
				
		从上面的例子我们可以发现，这组函数像极了JavaScript中的函数，它可以被调用和传递参数。
		这些函数的主要作用是提供颜色变换的功能，先把颜色转换成HSL色，然后在此基础上进行操作。
		LESS提供的运算及函数特性适用于实现页面组件特性，比如组件切换时的渐入渐出。

### Comments（注释）

		适当的注释是保证代码可读性的必要手段，LESS对注释也提供了支持，主要有两种方式：单行注释和多行注释，
		这与JavaScript中的注释方法一样，我们这里不做详细的说明，
		只强调一点：LESS中单行注释（// 单行注释）是不能显示在编译后的CSS中，
		所以如果你的注释是针对样式说明的请使用多行注释。
