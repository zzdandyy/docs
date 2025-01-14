---
description: '2021-02-05'
---

# day12

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### Bootstrap

前端开发框架，响应式布局

初始模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <!-- 这3个meta标签在最前面-->
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>初始模板</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
<body>

<h1>hello world</h1>

</body>
</html>
```

响应式布局

栅格系统：一行平均分成12个格子，可以指定元素占几个格子

* 定义容器，相当于table
  * container
  * container-fluid
* 定义行，相当于tr 样式：row
* 定义元素，相当于td，指定该元素在不同的设备上占格子的数目   样式：col-设备代号-格式数目
  * xs：手机&lt;768px    :col-xs-12
  * sm: 平板&gt;768px   
  * md: 桌面显示器 &gt;992px
  * lg: 大屏幕 &gt;1200px

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <!-- 这3个meta标签在最前面-->
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>栅格系统</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>

    <style>
        .inner {
            border: 1px solid red;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="row">
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 col-xs-4 inner">栅格</div>
    </div>
</div>

</body>
</html>
```

全局css样式

* 按钮
  * class="btn btn-default"
* 图片
  * class="img-responsive"
* 表格
  * class="table"
* 表单
  * ```html
    <form>
      <div class="form-group">
        <label for="exampleInputEmail1">Email address</label>
        <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">Password</label>
        <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
      </div>
      <div class="form-group">
        <label for="exampleInputFile">File input</label>
        <input type="file" id="exampleInputFile">
        <p class="help-block">Example block-level help text here.</p>
      </div>
      <div class="checkbox">
        <label>
          <input type="checkbox"> Check me out
        </label>
      </div>
      <button type="submit" class="btn btn-default">Submit</button>
    </form>
    ```

组件

* 导航条
* 分页条

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <!-- 这3个meta标签在最前面-->
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>组件</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
<body>

<nav class="navbar navbar-inverse">
    <div class="container-fluid">
        <div class="navbar-header">

            <!-- 定义汉堡按钮 -->
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>

            <!-- 左上角的logo-->
            <a class="navbar-brand" href="#">Brand</a>
        </div>


        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">

            <ul class="nav navbar-nav">

                <!--两个页面-->
                <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                <li><a href="#">Link</a></li>

                <!--下拉列表-->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>


            </ul>

            <!-- 搜索栏-->
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>


            <ul class="nav navbar-nav navbar-right">
                <!-- 左上角的页面-->
                <li><a href="#">Link</a></li>

                <!--右上角的下拉列表-->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>

                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
<nav aria-label="Page navigation">
    <ul class="pagination">
        <li>
            <a href="#" aria-label="Previous">
                <span aria-hidden="true">&laquo;</span>
            </a>
        </li>
        <li><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <li><a href="#">5</a></li>
        <li>
            <a href="#" aria-label="Next">
                <span aria-hidden="true">&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
</body>
</html>
```

插件

* 轮播图

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <!-- 这3个meta标签在最前面-->
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>插件</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
<body>

<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
    <!-- Indicators -->
    <ol class="carousel-indicators">
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
        <li data-target="#carousel-example-generic" data-slide-to="2"></li>
    </ol>

    <!-- Wrapper for slides -->
    <div class="carousel-inner" role="listbox">
        <div class="item active">
            <img src="..." alt="...">
            <div class="carousel-caption">
                ...
            </div>
        </div>
        <div class="item">
            <img src="..." alt="...">
            <div class="carousel-caption">
                ...
            </div>
        </div>
        ...
    </div>

    <!-- Controls -->
    <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
        <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>
    </a>
    <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
        <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
        <span class="sr-only">Next</span>
    </a>
</div>

</body>
</html>
```

