# Listeners

laminas-mvc-console registers a number of listeners with laminas-mvc applications.
Below is a list of events, and the listeners laminas-mvc-console registers.

## `MvcEvent::EVENT_BOOTSTRAP` ("bootstrap")

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

Class                               | Priority | Method Called | Triggers | Description
------------------------------------|---------:|---------------|----------|------------
`Laminas\Mvc\Console\View\ViewManager` | 10000    | `onBootstrap` | none     | Prepares the view layer.

### Triggered By

This event is triggered by the following classes:

Class                  | In Method
-----------------------|----------
`Laminas\Mvc\Application` | `bootstrap`

## `MvcEvent::EVENT_DISPATCH` ("dispatch")

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

Class                                                    | Priority | Method Called               | Description
---------------------------------------------------------|---------:|-----------------------------|------------
`Laminas\Mvc\Console\View\InjectNamedConsoleParamsListener` | 1000     | `injectNamedParams`         | Merge all params (route match params and params in the command), and add them to the `Request` object.
`Laminas\Mvc\Console\View\CreateViewModelListener`          | -80      | `createViewModelFromArray`  | If the controller action returns an associative array, this listener casts it to a `Laminas\Mvc\Console\View\ViewModel` object.
`Laminas\Mvc\Console\View\CreateViewModelListener`          | -80      | `createViewModelFromString` | If the controller action returns a string, this listener casts it to a `Laminas\Mvc\Console\View\ViewModel` object.
`Laminas\Mvc\Console\View\CreateViewModelListener`          | -80      | `createViewModelFromNull`   | If the controller action returns null, this listener casts it to a `Laminas\Mvc\Console\View\ViewModel` object.
`Laminas\Mvc\Console\View\InjectViewModelListener`          | -100     | `injectViewModel`           | Inserts the `ViewModel` (in this case, a `Laminas\Mvc\Console\View\ViewModel`) and adds it to the `MvcEvent` object. It either (a) adds it as a child to the default, composed view model, or (b) replaces it if the result is marked as terminal.

### Triggered By

This event is triggered by the following classes:

Class                                    | In Method   | Description
-----------------------------------------|-------------|------------
`Laminas\Mvc\Application`                   | `run`       | Uses a short circuit callback to halt propagation of the event if an error is raised during routing.
`Laminas\Mvc\Controller\AbstractController` | `dispatch`  | If a listener returns a `Response` object, it halts propagation. Note: every `AbstractController` listens to this event and executes the `onDispatch` method when it is triggered.

## `MvcEvent::EVENT_DISPATCH_ERROR` ("dispatch.error")

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

Class                                           | Priority | Method Called               | Description
------------------------------------------------|---------:|-----------------------------|------------
`Laminas\Mvc\Console\View\RouteNotFoundStrategy`   | 1        | `handleRouteNotFoundError ` | Detect if an error is a "route not found" condition, and display a usage message.
`Laminas\Mvc\Console\View\ExceptionStrategy`       | 1        | `prepareExceptionViewModel` | Create an exception view model.
`Laminas\Mvc\Console\View\InjectViewModelListener` | -100     | `injectViewModel`           | Inserts the `ViewModel` (in this case, a `Laminas\Mvc\Console\View\ViewModel`) and adds it to the `MvcEvent` object. It either (a) adds it as a child to the default, composed view model, or (b) replaces it if the result is marked as terminable.

### Triggered By

Class                         | In Method
------------------------------|----------
`Laminas\Mvc\MiddlewareListener` | `onDispatch`
`Laminas\Mvc\DispatchListener`   | `onDispatch`
`Laminas\Mvc\DispatchListener`   | `marshallControllerNotFoundEvent`
`Laminas\Mvc\DispatchListener`   | `marshallBadControllerEvent`

## `MvcEvent::EVENT_RENDER` ("render")

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

Class                                            | Priority | Method Called | Description
-------------------------------------------------|---------:|---------------|------------
`Laminas\Mvc\Console\View\DefaultRenderingStrategy` | -10000   | `render`      | Render the view.

### Triggered By

This event is triggered by the following classes:

Class                  | In Method         | Description
-----------------------|-------------------|------------
`Laminas\Mvc\Application` | `completeRequest` | This event is triggered just before the `MvcEvent::FINISH` event.

## `MvcEvent::EVENT_RENDER_ERROR` ("render.error")

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

#### Console Context only

The following listeners are only attached in a console context:

Class                                           | Priority | Method Called               | Description
------------------------------------------------|---------:|-----------------------------|------------
`Laminas\Mvc\Console\View\ExceptionStrategy`       | 1        | `prepareExceptionViewModel` | Create an exception view model.
`Laminas\Mvc\Console\View\InjectViewModelListener` | -100     | `injectViewModel`           | Inserts the `ViewModel` (in this case, a `Laminas\Mvc\Console\View\ViewModel`) and adds it to the `MvcEvent` object. It either (a) adds it as a child to the default, composed view model, or (b) replaces it if the result is marked as terminable.

### Triggered By

This event is triggered by the following classes:

Class                                         | In Method | Description
----------------------------------------------|-----------|------------
`Laminas\Mvc\View\Http\DefaultRenderingStrategy` | `render`  | This event is triggered if an exception is raised during rendering.

## `SendResponseEvent::EVENT_SEND_RESPONSE`

### Listeners

The following classes listen to this event (sorted from higher priority to lower
priority):

Class                                                        | Priority | Method Called | Description
------------------------------------------------------------ | -------: | ------------- | -----------
`Laminas\Mvc\Console\ResponseSender\ConsoleResponseSender`      | -2000    | `__invoke`    | Emits console output.

### Triggered By

This event is triggered by the following classes:

Class                           | In Method         | Description
--------------------------------|-------------------|------------
`Laminas\Mvc\SendResponseListener` | `sendResponse`    | Triggered by `MvcEvent::FINISH` at a priority of -10000, this listener emits the response to the client.
