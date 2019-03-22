---
layout: post
title:  "前端 | Guidance for Angular 5"
date:   2018-07-24 14:00:00
categories: front guidance angular
tags:
- Guidance
---

Angular 是一个近乎全能的前端框架，可以用于开发较大规模的应用。虽然它前期学习的台阶比较多，但疏通逻辑之后就会赞叹其精彩完善的设计模式，十分值得掌握。

Angular 的早期版本（Angular 1）与后面的 2-5 版本有很大的差异。Angular 2-5 用的是 TypeScript ，它是 Javascript 的一个超集（拓展）。所以，官方现在把 Angular 2-5 统称为 Angular，而把 Angular 1 称为 AngularJS。本文学习的是 Angular 5。

## 基础准备

* node.js
* Angular Cli

Angular 为使用者们提供了一个命令行工具 Angular Cli。这个工具类似于批处理工具，能增加我们开发 Angular 应用的效率，也能帮我们避开学习 Angular 过程中很多可能不必要的麻烦。

为了安装 Angular Cli，我们需要较高版本的 node.js。

### node.js

最新版的 node.js 可以直接从 [node.js官网][nodejs] 下载

### Angular Cli

有了 node.js 后，使用它的包管理工具 (npm) 全局安装 Angular Cli：

```terminal
$ npm install -g @angular/cli
```

> 上述命令在 cmd 中执行。其中 $ 号只是表示一行命令的开始，不需要被输入。

#### 卸载旧版的 Angular Cli （如果有）

如果有旧版 Angular Cli，应先卸载。

```terminal
$ npm uninstall -g @angular/cli
```

然后清空缓存，确保卸载完全。

* 高版本 npm 执行 `npm cache verify`
* 低版本 npm 执行 `npm cache clean`

## 理解 Angular

* MVC 设计模式
* 组件化和模块化思想

### MVC 设计模式

Angular 使用了 **M(odel)-V(iew)-C(ontroller)** 设计模式，这是 Angular 的一个核心理念，先大致地理解它，会对学习有很大帮助。

这个模式将应用分为三层，分别是模型层(Model)，视图层(View)和逻辑层(控制器,Controller)

* **M**odel
    * 模型层，应用里所有**数据**的默认值、结构和处理方法，都是在 Model 中得到管理。一个 Model 封装了我们所需要的一套数据和相应的一套函数方法，每个 Model 都可以在很多个 View （视图）中被重复使用。
    * 比如我们可以抽象出一个"歌"的 Model，规定
        * 一首歌要具备歌名、歌手和点赞数三个属性（这是数据的**结构**）
        * 点赞数默认为 0（这是数据的默认值）。
        * 如果有人给这首歌点赞，就给这首歌的点赞数增加1（这是规定了处理**方法**）。
    * 我们使用这个 Model，传入参数 `("告白气球","周杰伦",100000)` 后，抽象的 *"歌"模型* 被实例化为周杰伦唱的《告白气球》这首歌，点赞数为 10 万。再通过一个*视图*，就可以把这些信息展现在应用程序界面上。
* **V**iew
    * 视图层，通俗地理解就是**外观**层。管理着展现给用户的页面外观，包括内容、布局、样式等。也就是 html 和 css 所在的层。
    * 视图的作用就是将数据信息最终展示出来，并且为用户操纵应用（改变数据）提供了交互途径（尽管**真正的数据处理发生在模型层**）。
    * 可以看到，视图仅仅负责外观展现和用户交互，所以应该尽量精简，把复杂、庞大的函数处理逻辑交给模型层。
* **C**ontroller
    * 逻辑层，也叫控制器，是事件处理函数所在的层，规定了 Model 和 View 之间如何沟通、事件如何**处理**。
    * 比如用户在《告白气球》这首歌的视图中点击了"点赞"按钮，那么控制器就应该调用 "歌"模型中写好的"点赞"方法，结果《告白气球》对应的实例化模型的"点赞数"就增加了1。
    * 逻辑层规定了"如果发生点击事件，就调用点赞方法"这一逻辑规则，实现了事件处理和视图层和模型层之间的数据交互。

理解了 Angular 的 MVC 理念后，在以后的设计中，还应记住一个原则：

* **Fat Models, Skinny Controllers**: We want to move most of our logic to our models so that our components do the minimum work possible.

### 组件化和模块化思想

#### 模块

一个大型 Angular 应用由许多个模块(Module)组成，每个模块负责不同的功能，比如登录功能和下载功能一般在不同的模块中。

#### 组件

一个模块是由几个 Angular 组件构成的。

Angular 组件可以理解为封装好的、相对独立的、可以重复利用的一块积木，这块积木有它自己的颜色(外观)、形状(结构)和含义(在页面上负责展示的信息)。

可以用好几块相同的小积木拼成一块大积木，好几块不同的大积木互相配合，可以实现一个更大的功能，也就是形成了一个模块。

组件是一个应用里最基本的单位，单个组件拥有自己的视图 (View)，**使用**特定的模型 (Model)，拥有自己的一套处理逻辑 (Controller)。

组件只是负责调用模型层的数据和方法，把传入组件的数据值填充在视图层，并展示出来（或者通过用户在视图层的操作，去调用相应模型层的方法处理数据），所以组件是抽象的、可复用的。可见，组件化避免了重复冗余的代码和不必要的工作量，是一个精妙的设计。

#### 组件组成模块，模块构成应用

一个应用可以拆分成不同模块，每个模块使用了不同的组件，最终可以拆分成许多个小组件。我们从独立的、可复用的小组件写起，最终拼接成一个大规模应用，这就是组件化和模块化思想。

例子：
* 我们用一个小组件来展示单独一首歌的信息，我们可以给这首歌点赞。
* 向相同的小组件填入不同的数据，可以展示不同歌的信息
* 这些相同的小组件拼在一起，就形成了一个歌曲列表。所以歌单是一个等级更高的组件（父组件），它使用了很多"歌"组件来组成自己。我们可以在歌曲列表里给不同的歌点赞，处理数据的方法是一样的。
* 歌曲列表和封面，这两个大的组件可以组成一个"专辑"，所以专辑是一个更高级的组件。
* 许多不同的专辑构成了专辑列表，再加上搜索框等等，最终形成了音乐 App 里的"专辑"模块，专门负责专辑的功能。

#### 组件、模块和模型的代码实现

Angular 的组件、模块和模型都用 TypeScript 语法实现，文件的后缀都不是 .js 而是 .ts。

TypeScript 是 JavaScript 语法的拓展，大部分语句其实是相似的。顾名思义，TypeScript 就是为每个变量赋予一个特定类型，再按类型来操作变量的语法，这就和 JavaScript 的理念有所区分。

在代码中，Angular 的组件、模块和模型都是一个类(class)，但是使用了不同的 *装饰器* 。接下来会分别展示它们不同的代码结构，但首先要理解 *装饰器* .

##### 装饰器

顾名思义，装饰器起到的是"点缀"的作用，它为一个变量写入了一些 *元数据* ，元数据是变量的基本信息。装饰器以 @ 开头，传入元数据。装饰器写在变量前面，和变量之间间隔一个空格即可。

带有装饰器 (如@YourDecorator) 的类 (类也是一个变量) 的基本语法为：

```typescript
@YourDecorator(...)
class yourClassName {...}
```

比如我们生产了一瓶绿茶，在装饰器里就可以写入它的品名、生产商和保质期，方便超市使用这些信息。

我们要写入的信息是一个对象，`{ name:'绿茶', factory:'健师傅', date:'2019/06/01' }`，假设这个产品的装饰器是 @Production ，定义绿茶饮料的类为 greenTea ，我们就可以向这个特定的 greenTea 写入元数据。

```typescript
@Production({ 
    name:'绿茶', 
    factory:'健师傅', 
    date:'2019/06/01' })
class greenTea {
    ...
}
```

##### 先引入装饰器

要在一个 ts 文件中使用装饰器，必须先在文件开头 import 这个装饰器，比如要使用组件装饰器，应该在文件开头写

```typescript
import { Component, OnInit } from '@angular/core';
//Component 和 OnInit 的使用下面会解释
```

##### 组件的代码实现

一个组件的类被一个装饰器 @Component 装饰，比如 ArticleComponent 组件：

```typescript
@Component({
  selector: 'app-article',
  templateUrl: './article.component.html',
  styleUrls: ['./article.component.css']
})
export class ArticleComponent implements OnInit {
  //...声明组件的一些特定属性(可以是一个模型)。可以在这里调用模型层的模型。
  constructor() {
    //...初始化
  }
  //...编写组件的交互逻辑
  ngOnInit() {
  }
}
```

* **装饰器 @Component** 表示这个类是一个 Angular 组件
* 注意组件是可复用的，所以它本质上是一个类，只是被 @Component 装饰了。
* 装饰器里为这个组件传入了三个元数据
    * **templateUrl** 是这个组件的**视图**所在的 html 文件路径，编辑该 html 文件就可以改变组件的**外观**。
    * **styleUrls** 是这个组件的**视图**所在的 css 文件路径，这个 css 文件只对这个组件（类）起作用
    * **selector** 是这个组件的标签选择器(**标签名**)。也就是说，要想渲染这个组件，只需要在父组件的视图 (html文件) 中使用子组件元数据里规定的标签 `<app-article></app-article>` 就可以了
* 装饰器的后面声明了一个类，并且导出(export)这个类，这样这个类就可以被另一个文件 import 进来使用。
* 这个组件类继承了 Angular 封装好的 OnInit 类，这是组件的一个基础类，可以实现其一些基础功能。不但继承了 OnInit 类的基本组件功能，还为了实现自身特定的功能而进行了拓展（implememts），最终形成 AritcleComponent 组件类。
* **constructor** 是构造器函数，每当这个组件被生成时，这个函数就被调用一次。
* ngOnInit 是 Angular 组件的生命周期函数，将在这个组件初始化时被调用一次。
* MVC 中，一个组件的**逻辑层(Controller)**就写在类里，**视图层(View)**写在装饰器中

##### 模块的代码实现

一个 Angular 模块也是一个类，被装饰器 @NgModule 装饰了。

```typescript
@NgModule({
  declarations: [
    AppComponent,
    ArticleComponent,
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

* 模块也是一个类，类名为 AppModule ，这个类同样被导出 (export) 用于构建应用
* 模块类**使用 @NgModule 装饰**，装饰器写入了四条元数据
    * **declarations** 声明了这个模块包含的所有组件，这些组件相互配合，共同实现了模块的功能
    * **imports** 说明这个模块使用了另一个模块的功能，这里引入的模块名为 BrowserModule
    * **providers** 是这个模块里需要使用的 *服务* 名称。服务是一个类似组件的函数概念，用于实现特定的功能，比如从服务器请求某组数据，也是我们自己写的。组件在实现自己功能的过程中，可能需要调用到这些服务。
    * **bootstrap** 是根组件的名称，说明了这个模块启动时需要首先启动哪个**最顶层**的父组件。由这个父组件再去使用其他子组件，最终把整个模块渲染出来。
    * 四条元数据的值都是数组

##### 模型的代码实现

模型层里的模型封装了数据的结构和方法，是 Angular 应用里最庞大的一块。

在 Angular 里，**没有装饰器的类**就是模型。一般，模型的文件需要我们自己创建（Angular Cli 帮不了忙），文件名一般命名为 yourComponentName.model.ts，表明是某个组件使用的模型。文件内部只需要导出 (export) 一个没有装饰器的类就可以了。

比如一个 "歌" 模型，留意注释部分：

```typescript
export class Song {
    //声明这个模型的三个属性（歌名、歌手、点赞数）
    title: string;
    singer: string;
    votes: number;

    //外部使用"歌模型"时，只需要向模型依次传入相应数据，
    //模型的构造器(constructor)就会将数据自动初始化成这首歌的三个属性
    constructor(title: string, singer: string, votes?:number) {
        //构造函数要求至少两个参数（歌名、歌手），点赞数可以不传入
        this.title = title;
        this.singer = singer;
        this.votes = votes || 0; //如果外部没有传入点赞数(即 votes 未定义)，就默认设为 0
    }

    //接下来是这个模型的内部处理方法

    //"点赞"方法可以让这首歌的点赞数加1
    voteUp():void {
        this.votes +=1
    }
    //"取消赞"方法可以让这首歌的点赞数减1
    voteDown():void {
        if (this.votes > 0){
            this.votes -=1
        }
    }

    //这些函数同样有 TypeScript 的特点: 先声明这个函数返回的是空值 void ，再定义函数的逻辑。
}
```

组件使用这个模型时，只需要把具体的数据填入模型，就可以将抽象的、可复用的模型实例化：

```typescript
//实例化，使用"歌模型"生成了两首歌
song1 = new Song("告白气球","周杰伦",100000)
song2 = new Song("如约而至","许嵩") 

//逻辑层调用点赞方法给"告白气球"点赞，
//逻辑层并不需要知道怎么处理数据，它只需要调用"歌"模型的"点赞"方法
song1.voteUp();
```

### Angular 交互的基本逻辑

最后，简单描述 Angular 应用与用户交互的基本逻辑，我们就大致理解 Angular 的所有核心理念了。

小张在音乐软件上听到一首很喜欢的歌"告白气球"，于是给那首歌点了个赞，接下来发生了什么？

在**视图层**(View)，侦测到一个按钮发生了点击事件，这个按钮组件的视图长这样：

```
<button (click)="anUserVotesUp()">为这首歌点赞</button>
```

* **(click)** 的括号表示监听了这个 `<button>` 元素的点击事件
* **(click)="anUserVotesUp()"** 表示当侦测到用户对这个 `<button>` 元素的点击时，调用逻辑层的函数 `anUserVotesUp()`

在**逻辑层**(Controller)，已经写好了交互逻辑 `anUserVotesUp()`，它长这样：

```typescript
anUserVotesUp(){
    this.song.voteUp();
    return false;
}
```

* **this.song** 是指"歌组件"的 song 属性
* 在《告白气球》这个"歌组件"生成的时候，组件就调用了模型层的"歌模型"，形成了一个特定的实例"告白气球"，作为"告白气球"歌组件的 *属性*。
* 这个实例化的"歌模型"包含了"告白气球"的所有信息和所有方法。
* 当小张点击"点赞"按钮时，Angular 就根据**逻辑层**里写好的规则，去调用**模型层**里写好的"点赞"方法

**模型层**内部的"点赞"方法真正处理了数据，把"告白气球"的点赞数增加 1

```ts
voteUp():void {
    this.votes +=1
}
```

* 这里的 this 是指模型层的 Song 模型，模型也是一个类，所以也可以说 this 指的是 Song 类（class Song）

在模型层更新了点赞数后，视图层的点赞数也同时增加了，因为视图层展示的数据是以模型层为基础的。视图层的展示部分长这样：


\<label\> \{\{ song.votes \}\} \</label\>


* **双大括号 \{\{\}\} 表示页面的数据绑定自模型层**，模型层的数据如果发生变化，页面的数据也会相应变化
* song 是歌组件的属性。在"告白气球"组件中，它是一个实例化的歌模型。
* songs.votes 表示引用了"告白气球"歌模型的 votes 属性，将值展示在 `<label>` 里

这样就完成了 Angular 应用的一个基本交互。

### 更进一步：从歌组件到歌曲列表

#### 实现用户交互的歌组件代码

在上文中，小张点赞后，由歌组件完成了一首歌的外观更新和交互逻辑。歌组件是一个用 @Component 装饰的组件类。

最终版本的歌组件长这样：

```typescript
//这是一个歌组件
@Component({
  selector: 'app-song', //组件的标签名，下面父组件会用到它
  templateUrl: './song.component.html', //刚才的点赞按钮（视图层）就写在这里
  styleUrls: ['./song.component.css']
})
export class SongComponent implements OnInit {
    @Input song: Song; //先声明组件的 song 属性是一个实例化的"歌"模型，这是TypeScript的语法特点
    //上一行使用了装饰器 @Input，表示这个歌组件的 song 属性是由外部(父组件)传入的

    constructor() {
    }

    //------------这里是组件的逻辑层-----------------//
    anUserVotesUp(){
    this.song.voteUp();
    return false;
  }
}
```

#### 抽象化:可重复利用的歌组件

父组件是歌曲列表，在父组件的逻辑层定义一列歌数组：

```ts
//先声明属性 song 的类型 Song[]
songs: Song[]; //Song 类型来自于模型层的 "歌模型"，后面的[]表示这是一个由歌模型组成的数组

songs = [new Song("告白气球","周杰伦"),
        new Song("柳成荫","许嵩"),
        new Song("盲点","G.E.M")]
```

使用歌组件标签`<app-song>`，在父组件的 *视图* 中渲染出一首 "告白气球"：

```
<app-song [song]="songs[0]"></app-song>
```

* [song] **中括号 [] 表示向子组件传入（并绑定）属性**，向歌组件(SongComponent)传入名为 song 的属性
* [song]="song[0]" 表示传给歌组件的 song 属性的值是 *父组件的一个变量* songs[0]。
* 子组件的**装饰器 @Input()**从父组件获取（并绑定）了自身 song 属性的值，即告白气球的"歌模型"。`@Input() song:Song`

> 因为使用了装饰器 @Input，所以在子组件的 ts 文件开头，同样需要先引入它
> 
> import { Input } from '@angular/core'

这样，歌组件自身不对数据进行赋值，只对数据进行展示，数据具体的值由父组件传入，实现了歌组件的抽象化，现在歌组件可以被重复利用，不需要为另一首歌再写一份相似的代码。换句话说，歌组件现在可以重复利用了。

#### 不止于一首歌

在实际运用中，一般在父组件生成一个"歌"数组，用循环的方法给每个"歌组件"传入不同的属性。

如何循环？使用 Angular 的 NgFor 指令即可，只需小改父组件（歌曲列表）的视图：

```
<app-song [song]="oneSong" *ngFor="let oneSong of songs"></app-song>
```

* `*ngFor` 表示循环渲染 *ngFor 指令所在的 html 标签
* `let oneSong` 是一个引用变量，歌数组 songs 里面的每一个元素都会生成一个变量 oneSong
* `*ngFor="let oneSong of songs"` 表示遍历歌数组 songs 里面的所有歌模型，每一个歌模型 oneSong 都传给一个歌组件的 song 属性。

这样就可以循环渲染出三个歌组件，分别是不同的歌曲，形成一个歌曲列表了。

现在，对于 Angular 的核心理念和基本使用方法，我们其实已经有很多的了解了，如果尚未理解，可在实践中体会。

---

以上是对 Angular 基础的一点理解和总览，剩余内容是我在学习 Angular 过程中的部分学习笔记，有助于掌握 Angular 框架的基础。更深入的学习可以研究[官方文档][angular]，个人体会，更重要地，实践出真知啊。

> 个人感觉，英文有时比中文更能准确地传达一些意思，所以，当感觉用英文表述着更顺畅时，就用英文书写了。

## BASIC USAGE

* 创建一个 Angular 应用
* 启动开发服务器
* 创建一个 Angular 组件

### 创建一个 Angular 应用

创建一个全新的 Angular 应用，应用名称（也是文件夹名）比如为 angular-hello-world

```terminal
$ ng new angular-hello-world
```

### 启动开发服务器

```terminal
$ cd angular-hello-world

$ ng serve
```

So that the NG Live Development Server will be running on *http://localhost:4200* (default port is 4200). Now we can open a browser and visit it.

If everything goes well, you will see an index page with successful infomation like "Angular app works!" on the top.

Or calling `ng serve --port 1234` will let the server run on *http://localhost:1234* .

You can also add param `--open` after `ng serve` and a space , it will open a browser window automatically. 

### 创建一个 Angular 组件 (Component)

在应用根目录 angular-hello-world/ 中使用 Angular cli 提供的命令，可以很方便地创建一个组件

```terminal
$ ng generate component hello-world
```

> You may have to open another terminal to call the code above.

Angular 应用自带一个根组件（就是最高等级的组件）AppComponent。在父组件的视图层中使用子组件的标签，才可以在网页中看到子组件。

### 循环生成 html 元素

* `*ngFor`是循环指令
* `let individualUserName` 是一个引用，说明引用后面数组里的每一个元素，并为每一次循环出的html元素分别生成一个变量 `individualUserName`

```
//Inside component class, let
names: string[]
//Inside the constructor, let
this.names = ['Appleman','Bananaman']
//In template, write
<ul>
    <li *ngFor="let individualUserName of names">Hello {{individualUserName}}
</ul>
```

#### 模板字符串

上例中 `{{ individualUserName }}`是一种模板字符串，可以直接用在属性值里：

```jsx
<a href="{{theLinkVariable}}">The Link</a>
```

另一种模板字符串用波浪号下方的反引号包裹，是ES6特性，可以用于输出多行字符串：

```jsx
`This is an example of Theory ${showing.count}`
//假设showing是已经定义的一个对象，有属性值count.
```

### 向组件传入属性值

```ts
<ul>
    <li><app-user-item [name]="individualUserName"></app-user-item></li>
</ul>
//In user-item.component.ts, use @Input
import {..., Input} from '@angular/core';
//in component class, let 'name' as an imported property,
@Input() name: string;
```

### 监听一个事件

#### 为 html 元素绑定一个事件

使用圆括号把事件括起来。

```ts
<button (click)="addArticle()" class="...">Submit Link</button>
```

#### 在控制器中定义回调函数

**Define the callback function** (e.g.addArticle()) on the component class.

```jsx
@Component({
  //...
})
export class AppComponent {
  addArticle(title: HTMLInputElement, link: HTMLInputElement): boolean{
    console.log(`Adding article title: ${title.value} and link: ${link.value}`);
    return false; //important
  }
}
```

* 回调函数中返回 false ，可以确保 event 不会被传播到父元素中（默认返回true，即"传播"）。

#### 为空链接标签`<a href (click)="callbackfun1()"></a>`绑定点击事件

**必须在 click 回调函数的最后 return false**，否则将 Angular 将刷新整个界面。
> **JavaScript, by default, propagates (v.传播) the click event to all the parent components.** To fix that, we need to **make the *click* handler to return *false***. This will make sure the browser won't try to refresh the page.

不局限于空链接标签，最好在每一个点击事件回调中都这样做。

### 表单传值

关键语法 `#yourVarName` 会生成为一个变量，这个变量是 `#yourVarName` 所在的整个html元素

```
<div class="field">
    <label for="title">Title:</label>
    <input name="title" #newtitle> <!--here is the change-->
</div>
<div class="field">
    <label for="link">Link:</label>
    <input name="link" #newlink> <!--here is the change-->
</div>

<button (click)="addArticle(newarticle,newlink)"> <!--here is another change-->
    Submit Link
</button>
```

组件类中函数的定义同上一条。

### 为一个组件的顶层元素 (host) 设置属性值

这里的顶层元素指组件的装饰器`@Component`中定义的`selector`对应的那个 html 标签，这个标签显然不会出现在组件自身的模板 (template) 里，因此只能用另一个装饰器`@HostBinding()`来声明其属性值。

这样做的好处是不需要在父组件里配置子组件的属性了。

一个组件的 ***host*** 是这个组件对应 (be attached to) 的 html 元素

要使用装饰器，必须先引入：

```
import {Component, HostBinding} from '@angular/core'
```

e.g. 我们想让组件 `ArticleComponent` 的顶层元素 `<app-article></...>` 的 `class` 属性中带有 `row` 

```ts
@Component({
  selector: 'app-article',
  //...
})
export class ArticleComponent implements OnInit {
  @HostBinding('attr.class') cssClass = 'row';
  //...
  constructor() {
    //...
  }
  ngOnInit() {
  }

}
```

### 在 Model 层定义数据结构

这个类只是一个单纯的类，它没有装饰器，也不是 Angular 的组件。

这样的类一般写在 `.model.ts` 文件中，因而是 MVC 模式中的模型层。

* 在constructor的变量名后面加一个 ? 号表示这个参数是可选的 (optional)。

示例如下
```
export class Article {
    title: string;
    link: string;
    votes: number;

    constructor(title: string, link: string, votes?:number) {
        this.title = title;
        this.link = link;
        this.votes = votes || 0;
        //如果 votes 未定义，则 this.votes 为 0
        //即：this.votes 默认为0
    }
}
```

#### 在函数中保证其封装性

一个 model 的数据具体怎么变化（函数方法），没必要让组件知道，组件想处理数据，只需要调用 model 的相应**方法**。因而数据的**算法**也应封装到 model class 里。

```
export class Article {
    title: string;
    link: string;
    votes: number;

    constructor(title: string, link: string, votes?:number) {
        //...
    }
    
    voteUp():void {
        this.votes +=1
    }

    voteDown():void {
        this.votes -=1
    }
}
```

实际应用中，函数方法可能更加复杂，比如需要请求一个外部API，因此把这些函数方法放在抽象、通用的 model 层里，而不是定义在每一个使用了 "model 实例" 的组件控制器里，是更明智的。

#### 如何使用模型层

将一个组件的属性值定义在 model 的一个实例中，而不是直接定义在组件内部。

需要先引入：

`import {Article} from './article.model'`

在组件中使用

```jsx
@Component({
  selector: 'app-article',
  //...
})
export class ArticleComponent implements OnInit {
  article: Article;

  constructor() {
    this.article = new Article(
      'Angular 2',
      'http://angular.io',
      10);
  }
  
  //调用model的内部方法来改变数据
  voteUp(){
    this.article.voteUp();
    return false; //返回 false 确保 event 不会被传播到父元素中，这个方法是控制器规定的而不是通用的，因而放在控制器里。
  }

  voteDown(){
    this.article.voteDown();
    return false;
  }

  ngOnInit() {
  }

}
```

## Reference

* [Angular Docs][angular]
* [ng-book - The Complete Book on Angular 5][ng-book2]

[nodejs]:https://nodejs.org/en/
[angular]:https://angular.cn/docs
[ng-book2]:https://www.ng-book.com/2/