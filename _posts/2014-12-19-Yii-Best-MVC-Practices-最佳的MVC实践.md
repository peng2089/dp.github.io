---
layout: post
title: Yii Best MVC Practices 最佳的MVC实践
keywords: MVC,Practices,Yii
description: 虽然几乎每个Web开发人员都知道MVC模型 - 视图 - 控制器（MVC），但是正确地在实际应用中使用它的可能寥寥无几。 MVC核心是代码的重用和关注点的分离。在本节中，我们描述了“如何在一个Yii应用中更好地遵循的MVC开发应用的”一般性的指导方针。
tags: [ 开发,Yii ]
---


Although Model-View-Controller (MVC) is known by nearly every Web developer, how to properly use MVC in real application development still eludes many people. The central idea behind MVC is code reusability and separation of concerns. In this section, we describe some general guidelines on how to better follow MVC when developing a Yii application.

> 虽然几乎每个Web开发人员都知道MVC模型 - 视图 - 控制器（MVC），但是正确地在实际应用中使用它的可能寥寥无几。 MVC核心是代码的重用和关注点的分离。在本节中，我们描述了“如何在一个Yii应用中更好地遵循的MVC开发应用的”一般性的指导方针。

To better explain these guidelines, we assume a Web application consists of several sub-applications, such as

> 为了更好地解释这些准则，我们假定Web应用程序由几个子应用，如

- front end: a public-facing website for normal end users;

> 前端：一个向最终用户展示的公共web站点

- back end: a website that exposes administrative functionality for managing the application. This is usually restricted to administrative staff;

> 后端：管理网站应用的管理平台。只有有权限的人能够访问使用。

- console: an application consisting of console commands to be run in a terminal window or as scheduled jobs to support the whole application;

> 控制台：运行在一个终端窗口的应用程序或作为支撑整个应用程序运行的后台任务;

- Web API: providing interfaces to third parties for integrating with the application.

> Web API：集成第三方应用接口或提供应用接口给第三方使用。

The sub-applications may be implemented in terms of modules, or as a Yii application that shares some code with other sub-applications.

> 这些子应用可能为其他模块提供接口或者是使用其他yii应用的一些代码。

## Model 模型

Models represent the underlying data structure of a Web application. Models are often shared among different sub-applications of a Web application. For example, a LoginForm model may be used by both the front end and the back end of an application; a News model may be used by the console commands, Web APIs, and the front/back end of an application. Therefore, models

> 模型是Web应用程序的底层数据结构。模型往往在不同的Web应用程序的子应用之间共享。例如，一个LoginForm的模型可能会被前端和后端应用程序使用;news模型可能同时被控制台程序，webAPIs，和应用程序的前端/后端使用。因此，模型应该有一下特点：

- should contain properties to represent specific data;

> 应提供属性来存储特定的数据;

- should contain business logic (e.g. validation rules) to ensure the represented data fulfills the design requirement;

> 应该包含业务逻辑（例如验证规则）确保数据的有效性，以及确保符合设计要求;

- may contain code for manipulating data. For example, a SearchForm model, besides representing the search input data, may contain a search method to implement the actual search.

> 可能包含操作数据的代码。例如，一个SearchForm模式，除了代表搜索输入数据，可能包含一个search方法来实现实际的搜索。

Sometimes, following the last rule above may make a model very fat, containing too much code in a single class. It may also make the model hard to maintain if the code it contains serves different purposes. For example, a News model may contain a method named getLatestNews which is only used by the front end; it may also contain a method named getDeletedNews which is only used by the back end. This may be fine for an application of small to medium size. For large applications, the following strategy may be used to make models more maintainable:

> 有时候，上述最后一条规则可能使一个模型变的冗长，在一个类中包含太多的代码。它也可能使模型难以维持，因为可能它包含的代码有不同的目的。例如，News模型可能包含名为前端只使用getLatestNews一个方法，它也可能包含名为getDeletedNews方法，为后端使用。对于这可能是中小规模的应用程序的可以理解。但是在意一个大型应用程序中就不适用了。可以使用以下策略，以使模型更易于维护：

- Define a NewsBase model class which only contains code shared by different sub-applications (e.g. front end, back end);

> 定义NewsBase模型类，只包含由不同的子应用程序（如前端，后端）共享的代码;

- In each sub-application, define a News model by extending from NewsBase. Place all of the code that is specific to the sub-application in this News model.

> 在每个子应用中，例如定义News模型继承NewsBase。将所有的和这个模型相关的代码，放在News模型中。

So, if we were to employ this strategy in our above example, we would add a News model in the front end application that contains only the getLatestNews method, and we would add another News model in the back end application, which contains only the getDeletedNews method.

> 所以，如果上面的例子采用这一战略，我们应在前端应用程序增加News模型仅包含getLatestNews方法，在后端的应用程序中，新建另一个News模型，其中包含只有getDeletedNews方法。

In general, models should not contain logic that deals directly with end users. More specifically, models

> 在一般情况下，模型不应该包含与最终用户直接交互的逻辑。更具体地说，模型应该具备如下特点：

- should not use $_GET, $_POST, or other similar variables that are directly tied to the end-user request. Remember that a model may be used by a totally different sub-application (e.g. unit test, Web API) that may not use these variables to represent user requests. These variables pertaining to the user request should be handled by the Controller.

> 不应该使用$ _GET，$ _POST或其他类似变量，例如直接关系到最终用户的请求的数据。请记住，一个模型可能是被用于多个完全不同的子应用（例如，单元测试，Web API），所以不能使用这些表示用户request的变量。和用户相关的变量，应当由控制器处理。

- should avoid embedding HTML or other presentational code. Because presentational code varies according to end user requirements (e.g. front end and back end may show the detail of a news in completely different formats), it is better taken care of by views.

> 应避免嵌入HTML或其他层（例如视图层）的代码。因为视图的代码会根据不同用户的需求而变化。（如前端和后端可能显示的信息的格式完全不一样）。这样的代码更为通用，可以提供给不同的代码层使用。



## View 视图层

Views are responsible for presenting models in the format that end users desire. In general, views

> 视图负责以最终用户期望的样式格式来显示其他模型的数据。一般情况下，视图应该具备如下特点：

- should mainly contain presentational code, such as HTML, and simple PHP code to traverse, format and render data;

> 应主要包含控制显示格式的代码，如HTML，和简单的PHP代码来遍历，格式和呈现数据;

- should avoid containing code that performs explicit DB queries. Such code is better placed in models.

> 应避免包含执行明确的数据库查询的代码。这样的代码是更好地放置在模型中。

- should avoid direct access to $_GET, $_POST, or other similar variables that represent the end user request. This is the controller's job. The view should be focused on the display and layout of the data provided to it by the controller and/or model, but not attempting to access request variables or the database directly.

> 应避免直接访问的$ _GET，$ _POST或其他类似变量，代表最终用户的请求的变量。这是控制器的工作。应侧重于视图，为控制器和/或模型提供的数据显示和布局，但不试图直接访问请求变量或数据库。

- may access properties and methods of controllers and models directly. However, this should be done only for the purpose of presentation.

> 当然也可以直接的使用控制器和模型的属性和方法。但是无论怎样这种用法仅用于演示目的。

Views can be reused in different ways:

> 视图层重用的几种方法

- Layout: common presentational areas (e.g. page header, footer) can be put in a layout view.

> Layout：一些公共的视图区域（例如页头，页脚）可以在布局视图中。

- Partial views: use partial views (views that are not decorated by layouts) to reuse fragments of presentational code. For example, we use _form.php partial view to render the model input form that is used in both model creation and updating pages.

> 使用局部视图仅限于（不受布局装饰限制）重用的视图代码片段。例如，在这两个模型的创建和更新页面中我们可以使用_form.php局部视图来呈现模型的输入形式。

- Widgets: if a lot of logic is needed to present a partial view, the partial view can be turned into a widget whose class file is the best place to contain this logic. For widgets that generate a lot of HTML markup, it is best to use view files specific to the widget to contain the markup.

> 部件Widgets：如果在局部视图中涉及到大量的逻辑，可以创建一个widget，其中来存放所有的处理逻辑。对于widget，产生了大量的HTML标记，最好是使用特定的widget来包含这些标记的视图文件。

- Helper classes: in views we often need some code snippets to do tiny tasks such as formatting data or generating HTML tags. Rather than placing this code directly into the view files, a better approach is to place all of these code snippets in a view helper class. Then, just use the helper class in your view files. Yii provides an example of this approach. Yii has a powerful CHtml helper class that can produce commonly used HTML code. Helper classes may be put in an autoloadable directory so that they can be used without explicit class inclusion.

> Helper类：在视图中，我们常常需要一些代码片段做微小的任务，如数据格式或生成HTML标记。这个代码直接放置到视图文件，不是一个更好的办法。最好的方式是把所有这些代码片断放在一个Helper类中。然后，只需在视图文件中使用这个helper类。 Yii提供了很多这种helper。 例如YII具有强大的CHTML的助手类，它可以产生常用的HTML代码。 Helper类，可在一个autoloadable目录中。

## Controller 控制器

Controllers are the glue that binds models, views and other components together into a runnable application. Controllers are responsible for dealing directly with end user requests. Therefore, controllers

> 控制器是把模型，视图和其它组件集成在一起的，形成一个可运行的应用程序。控制器负责处理最终用户的直接请求。因此，控制器应该具备如下特点：

- may access $_GET, $_POST and other PHP variables that represent user requests;

> 可使用$ _GET，$_POST和其他代表用户请求的PHP变量，;

- may create model instances and manage their life cycles. For example, in a typical model update action, the controller may first create the model instance; then populate the model with the user input from$_POST; after saving the model successfully, the controller may redirect the user browser to the model detail page. Note that the actual implementation of saving a model should be located in the model instead of the controller.

> 用于创建模型实例和管理其生命周期。例如，在一个典型的模型更新操作，控制器可先创建模型实例，然后获取用户输入的$ _POST数据，模型成功保存之后，控制器可能会重定向用户浏览器的显示模型的详细信息页面。需要注意的是实际执行处理过程应改位于模型中，而不是位于控制器中。

- should avoid containing embedded SQL statements, which are better kept in models.

> 应避免含有嵌入式的SQL语句，这些应该放在模型中。

- should avoid containing any HTML or any other presentational markup. This is better kept in views.

> 应避免包含任何HTML或任何其他的视图层的标记。这些应该放在视图层完成。


In a well-designed MVC application, controllers are often very thin, containing probably only a few dozen lines of code; while models are very fat, containing most of the code responsible for representing and manipulating the data. This is because the data structure and business logic represented by models is typically very specific to the particular application, and needs to be heavily customized to meet the specific application requirements; while controller logic often follows a similar pattern across applications and therefore may well be simplified by the underlying framework or the base classes.

> 在一个设计良好的MVC应用程序中，控制器，往往是非常简单的，可能只含有几十行的代码，而模型是非常庞大复杂的，含有大部分业务逻辑和操做数据的代码。这是因为数据结构和业务逻辑通常需要非常具体的，特定的应用，需要大量定制，以满足特定的应用需求，而往往控制器逻辑通常在大部分的应用程序中都是一个类似的模式，所以应当尽可能被简化底层框架或基类。


From: http://www.yiichina.com/guide/1/basics.best-practices