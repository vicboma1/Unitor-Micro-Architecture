# Framework Based On :
## Robotlegs (AS3) & Strange IOC & Dry-IO

# Framework 

Api
* [Context](https://github.com/vicboma1/FrameworkUnity/README.md#context)
* [Installer](https://github.com/vicboma1/FrameworkUnity/README.md#installer)
* [Configurator](https://github.com/vicboma1/FrameworkUnity/README.md#configurator)
* [Injector](https://github.com/vicboma1/FrameworkUnity/README.md#injector)
* [Dispatcher](https://github.com/vicboma1/FrameworkUnity/README.md#dispatcher)
* [Adapter](https://github.com/vicboma1/FrameworkUnity/README.md#adapter)
* [Controller](https://github.com/vicboma1/FrameworkUnity/README.md#controller)
* [Command Map](https://github.com/vicboma1/FrameworkUnity/README.md#command-map)
* [Reflector](https://github.com/vicboma1/FrameworkUnity/README.md#reflector)
* [Mono](https://github.com/vicboma1/FrameworkUnity/README.md#mono)
* [Patterns](https://github.com/vicboma1/FrameworkUnity/README.md#patterns)
* [Attributes](https://github.com/vicboma1/FrameworkUnity/README.md#attributes)
* [Errores Comunes](https://github.com/vicboma1/FrameworkUnity/README.md#errores-comunes)

## Context
```csharp
event EventHandler OnPostInitialize;
event EventHandler OnPreInitialize;
IInjector injector{ get; }
IContext Configure<T>() where T : class;
IContext PreInitialize();
IContext PostInitialize();
IContext AddConfigHandler(IMatcher matcher, Action<object> handler);	
IContext Install<T> () where T : IExtensionable ;
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-context)

## Installer
```csharp
void Install<T>() where T : IExtensionable;
void Clear();
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-installer)

## Configurator
```csharp
void AddConfig<T> () where T : class;
void AddConfigHandler(IMatcher matcher, Action<object> process);
void Destroy ();
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-configurator)

## Injector
```csharp
IInjector parent { get; set; }
IInjector CreateChild();
bool HasMapping <T>(object name = null);
InjectionMapping Map<T>(object name = null);
void Unmap<T>(object name = null);
bool Satisfies<T>(object name = null);
InjectionMapping GetMapping<T>(object name = null);
void Into(object target);
T GetInstance<T>(object name = null, Type targetType = null);
T GetOrCreateNewInstance<T>();
T InstantiateUnmapped<T>();
void DestroyInstance(object instance);
void Teardown();
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-injector)


## Dispatcher
```csharp
void Execute (IEvent evt);
void Add<T> (Enum type, Action<T> listener);
void Add (Enum type, Action<IEvent> listener);
void Add (Enum type, Action<IEventInjector> listener);
void Add (Enum type, Action<IEventContext> listener);
void Add (Enum type, Action listener);
void Add(Enum type, Delegate listener);
void Remove<T>(Enum type, Action<T> listener);
void Remove(Enum type, Action<IEvent> listener);
void Remove(Enum type, Action<IEventInjector> listener);
void Remove(Enum type, Action<IEventContext> listener);
void Remove(Enum type, Action listener);
void Remove(Enum type, Delegate listener);
void Clear();
bool Contains (Enum type);

IEvent { Enum type {	get; } }
IEventInjector : IEvent { IInjector injector { get;} }
IEventContext : IEventInjector { IContext context { get;} }
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-dispatcher)


## Adapter
```csharp
event EventHandler addEventHandler;
event EventHandler removeEventHandler;
event EventHandler disableEventHandler;
event EventHandler enableEventHandler;
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-adapter)

## Controller

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-controller)

## Command Map
```csharp
ICommandMapper Map<T>(Enum type);
ICommandUnMapper Unmap<T>(Enum type);
```
#### Dependencies
```csharp
ICommandUnMapper:
void FromCommand<T>();
void FromAll();

ICommandMapper:
ICommandConfigurator ToCommand<T>();

ICommandConfigurator:
ICommandConfigurator WithGuards(params object[] guards);
ICommandConfigurator WithHooks(params object[] hooks);
ICommandConfigurator Once(bool value = true);
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-command)

## Mono
```csharp
MonoParent: 
 static void ToValue(object _this);
 static void Into(object _this);
 static IContext GetContext();
 
MonoInject:
protected virtual void Awake();
protected virtual void Start();
[PostConstruct]public virtual void PostStart();
protected static IContext GetContext ();

MonoConfigurable:
public class MonoConfigurable : MonoInject , IConfigurable {
virtual void Configure();
}

MonoAdapterView:
public class MonoAdapterView : AdapterMonoView { }

MonoController:
public class MonoController : MonoInject { }

MonoController:
public class MonoManager : MonoInject { }

IMonoView:
object view { get; }
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-Mono)

## Reflector
```csharp
internal static T GetInstanceProperty(string _className, string propertyName);
internal static T GetInstanceField(object instance, string fieldName);
internal static T GetInstanceStaticMethod(Type instance, string fieldName);
internal static T GetInstanceStaticMethod(Type instance, string fieldName, object[] obj);
internal static T CreateInstanceDefaultExpressionLambda(Type instance);
internal static T CreateInstanceConstructor(Type instance);
internal static T CreateInstanceConstructor(Type instance, Type[] typesObject, object[] obj);
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-reflector)

## Patterns
```csharp
ICommand: 
void Execute(); 

IConfigurable: 
void Configure(); 

IDisposable:
void Dispose ();

IExtensionable:
void Extension(IContext context);

IPostConstructor:
[PostConstruct] void PostStart();

IUpdatable:
T Update<T>();
```

## Attributes

```csharp
public class ToInterface
public class ToTypeValue
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/README.md#ejemplo-attributes)


## Errores Comunes

#### Injector

* Cuando tenemos injecciones circulares, bidireccionales o cíclicas. 
Error de diseño.
Debemos de tener claro que el injector actúa de forma recursiva. Si "A" contiene a "B", "B" no puede contener el Inject de "A" porque entraríamos en un loop.
Lo solución es tener claro cual de las dos clases es la que tiene más fuerza. En este caso, si "A" contiene a "B", "B" puede recuperar a "A" mediante el injector. Éste está presente en todas las clases injectadas en la configuracion y accedemos a "A" a través de inject.GetInstance<A>() o al revés en un método que no sea ni el Awake() - Start () o [PostConstruct].

Podemos crear un metodo llamado "void Initialize()" y ser llamado desde fuera para recuperar esa dependencia.

```csharp
InjectorMissingMappingException: Injector is missing a mapping to handle injection into property 'XXXXXX' of object 'ZZZZZZ (ZZZZZZ)' with type 'ZZZZZZZ'. Target dependency: '[MappingId: type=XXXXXXX, key=]'
swiftsuspenders.typedescriptions.PropertyInjectionPoint.ApplyInjection (System.Object target, System.Type targetType, swiftsuspenders.Injector injector) (at /Users/james/Documents/git/swiftsuspenders-sharp/src/swiftsuspenders/typedescriptions/PropertyInjectionPoint.cs:31)
```

* Si utilizamos un mapeo en el injector "ToType" en el fichero de configuración obtendremos un error

```csharp
injector.Map<Figure>().ToType<Cuadrado>();
```
y el valor de la clase a recuperar mediante el inject será nulo. Solo se puede usar en el scope local de una llamada a un método/lambda y a través de "inject.GetOrNewCreateInstance<T>()"

```csharp
[Inject]
public Cuadrado cuadrado
```

#### Dispatcher

* Si queremos lanzar un Action sin el ICommander debemos de ejecutar la llamada con la instancia de un evento de contexto, evento de injection, evento de tipo o evento de T.

Añadimos
```csharp
dispatcher.Add (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, PuzzleControllerPostInitializeAction.Listener ());
```

Ejecutamos
```csharp
EventContext -> Obtiene el contexto 
EventInject -> Obtiene el injector
Event -> Obtine el Type
Event<T> -> Obtiene T

dispatcher.Execute (EventContext.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, context ));
dispatcher.Execute (EventInject.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, injector ));
dispatcher.Execute (Event.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION));
dispatcher.Execute (Event<T>.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, T ));

``` 

Muy atento a los parametros de la lambda.
Recuperamos un argumento de tipo Context.

```csharp
dispatcher.Execute (EventContext.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, context ));

public class PuzzleControllerPostInitializeAction {
	public static Action<IEventContext> Listener(){
		return (IEventContext eventContext) => {
			var _context = eventContext.context;
			};				
		};
	}
}
```

Recuperamos un argumento de tipo Inject.

```csharp
dispatcher.Execute (EventInject.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, injector ));

public class PuzzleControllerPostInitializeAction {
	public static Action<EventInject> Listener(){
		return (EventInject eventInjector) => {
			var _injector = eventInjector.injector;
			};				
		};
	}
}

``` 

Recuperamos un argumento de tipo Type.

```csharp
dispatcher.Execute (Event.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION ));

public class PuzzleControllerPostInitializeAction {
	public static Action<Event> Listener(){
		return (Event event) => {
			var _type = event.type;
			};				
		};
	}
}

``` 

Aquí es aconsejable pasarle un mapa siempre que se pueda.
```csharp
dispatcher.Execute (Event<T>.Create (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, T ));

public class PuzzleControllerPostInitializeAction {
	public static Action<Event<T>> Listener(){
		return (Event<T> event) => {
			var _T = event.T;
			};				
		};
	}
}

``` 


* Si queremos lanzar un ICommand debemos de ejecutar la llamada a través de la instancia de la clase del Evento. Ésta se injectará en el comando automáticamente.

```csharp
eventCommandMap.Map(PuzzleGameConfigureEvent.Type.POST_INITIALIZE_EVENT).ToCommand<PuzzleGameConfigureCommand>();
dispatcher.Add (PuzzleGameConfigureEvent.Type.POST_INITIALIZE_ACTION, TopRectangleScorePostInitializeAction.Listener ());
		
dispatcher.Execute(PuzzleGameConfigureEvent.Create());
``` 


## Ejemplo Context
El contexto será visible a través de la MetaData "[Inject] public IContext context;" en todas las clases que estén especificadas en el injector

```csharp
context = Context.Create ();
	 .Install<AdapterMonoView> ()
	 .Configure<PuzzleGameConfigure> ()
	 .Configure<XXXX> ()
	 .Configure<ZZZZ> ()
	 .PreInitialize();
	 .PostInitialize();
```

* Context.Create () crea el injector y añade las dependencias del configurator, eventDispatcher, eventCommandMap y el installador.

* PostInitialize() se llama cuando termina la carga de la Escena o el Start de todos los MonoBehaviour.

## Ejemplo Installer
Se llama al Pattern "void Extension(IContext context)" de la clase e injecta la dependencia "AdapterMonoView" en el injector. Es como una configuración independiente al Framework añadida al injector.
```csharp

context = Context.Create ();
	 .Install<AdapterMonoView> ();
	 
	 ----
	 
private IInjector injector;
private IMonoView monoView;
public void Extension (IContext context)
{
   injector = context.injector;
      if (context.injector.HasDirectMapping (typeof(IMonoView)))
      HandleContextView (context.injector.GetInstance (typeof(IMonoView)));
   else
    context.AddConfigHandler (
        new InstanceOfMatcher (typeof(IMonoView)), HandleContextView);
}
private void HandleContextView(object _newMonoView)
{
    var newMonoView = (IMonoView)_newMonoView;
    if (monoView != null || newMonoView == null)
    return;
   
   this.monoView = newMonoView;
   if (injector.HasDirectMapping (typeof(IAdapterView)))
   return;
   
   var viewStateWatcher = GetAdapterView(monoView.view);
   if (viewStateWatcher == null)
   {
      Debug.LogError ("No se puede crear la vista!.");
      return;
   }
   
   injector.Map(typeof(IAdapterView)).ToValue(viewStateWatcher);
}

```

## Ejemplo Configurator

Permite al contexto añadir una configuración donde se definen las especificaciones.
Se añaden sus dependencias a través del Pattern "void Configure();".
PreInitialize() inizialize la configuración y llama a Configure<T> que lanzará automáticamente el evento "OnPreInitialize". 


```csharp
context = Context.Create ();
	 .Configure<PuzzleGameConfigure> ()
	 .PreInitialize();
```

En la clase que tenemos implementado el Pattern "void Configure();" tenemos acceso a los injects y a las instalaciones del contexto

```csharp

public class PuzzleGameConfigure {
	
	[Inject] 
	public IInjector injector;

	[Inject] 
	public IContext context;

	[Inject] 
	public IEventDispatcher dispatcher;

	[Inject]
	public IEventCommandMap eventCommandMap;
	
	public override void Configure(){
		// Mi configuración....
	}
}
```

## Ejemplo Injector

El injector será visible a través de la MetaData "[Inject] public IInject injector;" en todas las clases que estén configuradas en el injector. 

Ejemplo
```
public class PuzzleGameConfigure {
	
	public override void Configure(){

		// To Value => Siempre la misma clase instanciada para el mismo tipo de dato.
		injector.Map<Touch>().ToValue (Touch.Create ());
		injector.Map<TopRectangleScoreModel> ().ToValue (TopRectangleScoreModel.Create ());
		injector.Map<PointCache>().ToValue (PointCache.Create ());
		injector.Map<LineCache>().ToValue (LineCache.Create ());
		injector.Map<LinkedNodeControllerCache>().ToValue (LinkedNodeControllerCache.Create ());
		
		var singletonHelloWorld = SingletonGameObject<HelloWorld>().Instance;
		injector.Map<HelloWorld>().ToValue(singletonHelloWorld)
		-> o usar una turbo cache en el editor y habilitar el SetActive del gameObject.


		// To Type => Instancias de clases prototipadas para usar dentro de un método; 
		// En el momento que la llamada acabe, se hace un UnMap de esta variable automáticamente en el injector.
		// Si se define la variable en el Scope de la clase, no funcionará o será null cuando la llames.
		//injector.Map<figure>().ToType<figure>();
		//injector.Map<figure>().ToType<Cuadrado>();

		//To Singleton => Crea directamente la instancia 
		injector.Map<Handler>().AsSingleton();
		//injector.Map<figure>().ToSingleton<Cuadrado>();
	}
```

## Ejemplo Dispatcher

Se puede usar sin un "ICommand" el Dispatcher para el uso de "IService"

Configuración:

```csharp	
public override void Configure(){
   dispatcher.Add (HelloWorldEvent.Type.END_COUNT_ACTION, HelloWorldAction.Listener());
   }
```

Event 
```csharp
public class HelloWorldEvent : Event
{
	public enum Type { HELLO_EVENT, HELLO_ACTION }
	public string Said { get; private set;}
	
	public static HelloWorldEvent Create(){
            return new HelloWorldEvent ();
	}

	HelloWorldEvent () : base(Type.HELLO_EVENT) {
	    Said ="Hello World!";
	}
}
```

Servicio:
```csharp
public class HelloWorldAction
{
    public static Action<IEventInjector> Listener () {
       return (IEventInjector eventInjector) => {
          var injector = eventInjector.injector;
          var helloWorldEvent = injector.GetInstance<HelloWorldEvent>();
          Debug.Log(helloWorldEvent.Said);
		};
	}
}
```

Lanzo el servicio:
```csharp
   dispatcher.Execute (EventInjector.Create (HelloWorldEvent.Type.HELLO_ACTION, injector ));
```
	
## Ejemplo Adapter

Definimos un GameObject en el Editor y le añadimos un Componente que extienda de "MonoAdapterView".
Automáticamente se añadirá al injector y tendremos acceso a las dependencias en el método [PostConstruct]public void PostStart().

```csharp
public class TopRectangleScoreAdapter : MonoAdapterView {

	[Inject]
	public IInjector vinjector;

	public Text countDownText;
	public Slider countDownGage;
	public float countDownTimeLength;

	public Text[] scoreTexts;

	protected override void Awake(){
		base.Awake ();
	}

	protected override void Start () {
		base.Start ();
	}
	
	[PostConstruct]
	public void PostStart(){
	}
		
	void Update () {
	
	}
}
```

## Ejemplo Controller

Definimos un GameObject en el Editor y le añadimos un Componente que extienda de "MonoController".
Automáticamente se añadirá al injector y tendremos acceso a las dependencias en el método [PostConstruct]public void PostStart().

```csharp
[Serializable]
public class NodeController: MonoController
{
	[Inject]
	public IInjector injector;
	
	[Inject]
	public MonoAdapterView NodeAdapter;

	public int id;
	public PuzzleController puzzleController { get; set;}
	public string _name;

	public NodeModel model;

	[Space(10)]
	public NodeAnimationController animationController;

	public Vector3 localPosition {
		get { return transform.localPosition; }
		set { transform.localPosition = value; }
	}
		
	protected override void Awake(){
		base.Awake ();
		model = NodeModel.Create ();
		animationController = NodeAnimationController.Create (this);
		_name = this.gameObject.name;
	}

	protected override void Start(){
		base.Start ();
	}

	[PostConstruct]
	public override void PostStart(){
	}	
}
```

## Ejemplo Command

Configuración:
```csharp
public override void Configure(){
  eventCommandMap.Map(CountDownTimerEndCountEvent.Type.END_COUNT_EVENT).ToCommand<CountDownTimerEndCountCommand>();
  dispatcher.Add (CountDownTimerEndCountEvent.Type.END_COUNT_ACTION, CountDownTimerEndCountAction.Listener());
}
```

Cuando lance el comando "UpdateScoreCommand" llamaré al evento "UpdateScoreEvent".
```csharp
   dispatcher.Execute(CountDownTimerEndCountEvent.Create());
```

Obligado que el "Type" esté dentro siempre del evento que vamos a lanzar. Está automatizado.

```csharp
public class CountDownTimerEndCountEvent : Event
{
	public enum Type { END_COUNT_EVENT, END_COUNT_ACTION }

	public static CountDownTimerEndCountEvent Create(){
		return new CountDownTimerEndCountEvent ();
	}

	CountDownTimerEndCountEvent () : base(Type.END_COUNT_EVENT) {
		
	}
}
```

Al comando se le pueden injectar las dependencias.
Dentro del comando harè un "dispatcher.Execute" de la accion "END_COUNT_ACTION". 
```csharp
using UnityEngine;
using System.Collections;

public class CountDownTimerEndCountCommand : ICommand {

	[Inject]
	public IInjector injector;

	[Inject]
	public IEventDispatcher dispatcher;

	[Inject]
	public CountDownTimerEndCountEvent countDownTimerEndCountEvent;

	[Inject]
	public PuzzleControllerUpdate puzzleControllerUpdate{ get; private set;}

	[Inject]
	public PuzzleConfiguration puzzleConfiguration{ get; private set; }

	public void Execute() {
		Debug.Log (" COMMAND Type.END_COUNT_ACTION *********************** ");

		//Game
		puzzleController.endMovingNodeModel ();

		puzzleConfiguration.isPause = true;
		puzzleConfiguration.count++;

		dispatcher.Execute (EventInjector.Create (CountDownTimerEndCountEvent.Type.END_COUNT_ACTION, injector ));
	}
}
```

Un "Action" es un servicio que hace algo.

```csharp
public class UpdateScoreAction
{
    public static Action<IEventInjector> Listener () {
       return (IEventInjector eventInjector) => {
          var injector = eventInjector.injector;
				//APP
		var managerDialog = injector.GetInstance<ManagerDialog>();
		managerDialog.SetActive (Constants.END_DIALOG, true);

		};
	}
}
```

## Ejemplo Mono
Clase que automatiza la llamada al contexto e injector de nuestros MonoBehaviour en el editor

```csharp
public class MonoInject : MonoBehaviour {
	protected virtual void Awake(){
		var _injector = GetInjector ();
		var _type = this.GetType ();
		if (!_injector.HasMapping (_type))
			_injector.Map (_type).ToValue (this);
	}

	protected virtual void Start(){
		GetInjector ().Into(this);
	}
		
	protected static IContext GetContext () {
		return Reflection<IContext>.GetInstanceProperty (Main.NAME, Context.NAME);
	}

	protected static IInjector GetInjector () {
		return GetContext().injector;
	}

	[PostConstruct]
	public virtual void PostStart(){

	}
}
```


## Ejemplo reflector
Libreria que permite técnicas de reflexión para invocaciones dinámicas a los Patterns del Framework, creaciones de clases con injects, setteo de variables privates, mocks, stubs...

https://github.com/vicboma1/Reflection


## Ejemplo attributes



```csharp
using UnityEngine;
using System.Collections;
using System.ComponentModel;

public interface Ixxxxxxxx {
   void Control();
}

[ToInterface(typeof(Ixxxxxxxx))]
public class Controller : MonoController , Ixxxxxxxx {

        [SerializeField]
        [ToTypeField(-10)]
        protected int a = 5;

        [SerializeField]
        [ToTypeField(-10)]
        protected int b = 5;

        [SerializeField]
        [ToTypeField(-10F)]
        protected float c = 888F;
     
        [ToTypeField(-10.222)]
	public double h;
     
        [ToTypeField("Hola")]
        public string str;

	[PostConstruct]
	public override void PostStart(){
	/ * ToTypeField availables!!! a = -10; | b = -10; | ... | str = "hola" */
		this.Control ();
	}
		
	public void Control(){
		Debug.Log ("I am a Controller!!!");
	}

	public void MonoControl(){
		Debug.Log ("I am a MonoControl!!!");
	}
}

using UnityEngine;
using System.Collections;

public class Controller2 : MonoController {

	[Inject]
	public Ixxxxxxxx controller;

	[PostConstruct]
	public override void PostStart(){
		Debug.Log ("I am a Controller 2");
		controller.Control ();
	}
		
}

```
