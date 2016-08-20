# Unitor - MicroFramework based on:
* Robotlegs (AS3)
* Strange IOC
* Dry-IO
* Promises (JS)
* Spring.io

Api
* [Context](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#context)
* [Installer](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#installer)
* [Configurator](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#configurator)
* [Injector](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#injector)
* [Dispatcher](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#dispatcher)
* [Adapter](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#adapter)
* [Controller](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#controller)
* [Command Map](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#command-map)
* [Reflector](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#reflector)
* [Mono](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#mono)
* [Graphic](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#graphic)
* [ResourceAsync](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#resourceasync)
* [Patterns](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#patterns)
* [Attributes](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#attributes)
* [Promises](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#promises)
* [Errores Comunes](https://github.com/vicboma1/Unitor-Micro-Architecture/blob/master/README.md#errores-comunes)

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

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-context)

## Installer
```csharp
void Install<T>() where T : IExtensionable;
void Clear();
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-installer)

## Configurator
```csharp
void AddConfig<T> () where T : class;
void AddConfigHandler(IMatcher matcher, Action<object> process);
void Destroy ();
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-configurator)

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
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-injector)


## Dispatcher
```csharp
void Execute (IEvent evt);
IPromise<T> ExecuteAsync (IEvent evt);
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

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-dispatcher)


## Adapter
```csharp
event EventHandler addEventHandler;
event EventHandler removeEventHandler;
event EventHandler disableEventHandler;
event EventHandler enableEventHandler;
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-adapter)

## Controller
```csharp
event EventHandler addEventHandler;
event EventHandler removeEventHandler;
IPromise<Enumerable<float>> Visible(Adapter adapter);
IPromise<Enumerable<float>> Hidden(Adapter adapter);
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-controller)

## Command Map
```csharp
ICommandMapper Map<T>(Enum type);
ICommandUnMapper Unmap<T>(Enum type);
```
#### Dependencies
```csharp
ICommandUnMapper:
void FromCommandAsync<T>();
void FromCommand<T>();
void FromAll();

ICommandMapper:
ICommandConfigurator ToCommand<T>();
ICommandConfigurator ToCommandAsync<T>();

ICommandConfigurator:
ICommandConfigurator WithGuards(params object[] guards);
ICommandConfigurator WithHooks(params object[] hooks);
ICommandConfigurator Once(bool value = true);
```
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-command)

## Mono
```csharp
MonoParent: 
 static void ToValue(object _this);
 static void Into(object _this);
 bool ToTypeField (object _this);
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

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-Mono)

## Graphic
```csharp	
IPromise<float> CrossFadeAlpha (Image image, float alpha, float duration, bool ignoreTimeScale);
IPromise<float> CrossFadeAlpha (Slider slider, float alpha, float duration, bool ignoreTimeScale);
IPromise<float> CrossFadeAlpha (Text text, float alpha, float duration, bool ignoreTimeScale);
IPromise<float> CrossFadeColor (CanvasRenderer canvasRenderer, Color targetColor, float duration, bool ignoreTimeScale, bool useAlpha, bool useRGB);
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-graphic)

## ResourceAsync
```csharp
static IPromise<Z> Execute(IContext context);
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-resourceasync)

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
[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-reflector)

## Patterns
```csharp
ICommand: 
void Execute(); 
IPromise<T> ExecuteAsync(); 

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

IFixedUpdatable:
T FixedUpdate<T>();
```

## Attributes

```csharp
class ToInterface
class ToTypeValue
class ToResourceAsync
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-attributes)

## Promises 

```csharp
IPromise<T>:
  void Done(Action<T> onResolved, Action<Exception> onRejected);
  void Done(Action<T> onResolved);
  void Done();
  IPromise<T> Catch(Action<Exception> onRejected);
  IPromise<Z> Then<Z>(Func<T, IPromise<Z>> onResolved);
  IPromise<T> Then(Action<T> onResolved);
  IPromise<Z> Then<Z>(Func<T, IPromise<Z>> onResolved, Action<Exception> onRejected);
  IPromise<T> Then(Action<T> onResolved, Action<Exception> onRejected);
  IPromise<Z> Then<Z>(Func<T, Z> transform);
  IPromise<IEnumerable<Z>> All<Z>(Func<T, IEnumerable<IPromise<Z>>> chain);
  IPromise<Z> Once<Z>(Func<T, IEnumerable<IPromise<Z>>> chain);
```

[Ejemplo](https://github.com/vicboma1/FrameworkUnity/blob/master//README.md#ejemplo-promises)

# Ejemplos

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
		get { return NodeAdapter.transform.localPosition; }
		set { NodeAdapter.transform.localPosition = value; }
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


Configuración Asíncrona:
```csharp
public override void Configure(){
  eventCommandMap.Map(CountDownTimerEndCountEvent.Type.END_COUNT_EVENT).ToCommandAsync<CountDownTimerEndCountCommandAsync>();
}
```

Cuando lance el comando "UpdateScoreCommandAsync" llamaré al evento "UpdateScoreEvent".
```csharp
   dispatcher
      .ExecuteAsync(CountDownTimerEndCountEvent.Create())
      .Then(()=> {
      	  Debug.Log("Obtengo la respuesta cuando acaba mi comando");
      });
```

Obligado que el "Type" esté dentro siempre del evento que vamos a lanzar. Está automatizado.

```csharp
public class CountDownTimerEndCountEvent : Event
{
	public enum Type { END_COUNT_EVENT }

	public static CountDownTimerEndCountEvent Create(){
		return new CountDownTimerEndCountEvent ();
	}

	CountDownTimerEndCountEvent () : base(Type.END_COUNT_EVENT) {
		
	}
}
```

La definicion del comando asíncrono nos obliga a implementar ICommandAsync

```csharp
using UnityEngine;
using System.Collections;

public class CountDownTimerEndCountCommand : ICommandAsync {

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

	public IPromise ExecuteAsync() {
	   var promise = new Promise();

	   Debug.Log (" COMMAND Type.END_COUNT_ACTION *********************** ");

	   //Game resolve the promise
	   puzzleController.endMovingNodeModel (promise);

	   puzzleConfiguration.isPause = true;
	   puzzleConfiguration.count++;
		    
	   return promise;
	}
}
```
El "Action se suprime y resolvemos la lógica del servicio en la resolución de la promesa
Un "Action" es un servicio que hace algo.

```csharp
 dispatcher
      .ExecuteAsync(CountDownTimerEndCountEvent.Create())
      .Then(()=> {
      	  managerDialog
      	      .SetActive (Constants.END_DIALOG, true)
      	      .Then((enumerable)=>{
      	            	  Debug.Log("Obtengo la respuesta");
      	      });
      });
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

## Ejemplo Graphic
Sobreescribe la clase abstract de UnityEngine para otorgar una promesa en los métodos asíncronos de ITweener

```csharp
Promise<float>.All(
	monoGraphic.CrossFadeAlpha (pauseDialogAdapter.textLogo,  alpha, 0.5F, true),
	monoGraphic.CrossFadeAlpha (pauseDialogAdapter.textBackground,  alpha, 0.5F, false)
).Then (enumerable => {
	promise.Resolve(enumerable);
});
```

Alternativa

```csharp
Promise<float>.All(
	pauseDialogAdapter.textLogo.CrossFadeAlpha(  alpha, 0.5F, true),
        pauseDialogAdapter.textBackground.CrossFadeAlpha(  alpha, 0.5F, false)
).Then (enumerable => {
	promise.Resolve(enumerable);
});
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

public interface ITopRectangleScoreAdapter {
	 Text countDownText { get; }
	 Slider countDownGage { get; }
	 Button countDownTimeButton{ get;}
}


[ToResourceAsync()]
[ToInterface(typeof(ITopRectangleScoreAdapter)]
public class TopRectangleScoreAdapter : MonoAdapterView, ITopRectangleScoreAdapter {

	[Inject]
	public IInjector vinjector;

	public Text _countDownText;
	public Slider _countDownGage;
	public Button _countDownTimeButton;

	public Text countDownText { get; private set;}
	public Slider countDownGage { get; private set;}
	public Button countDownTimeButton{ get; private set;}

	protected override void Awake(){
		base.Awake ();
	}

	protected override void Start () {
		base.Start ();
	}
	
	[PostConstruct]
	public void PostStart(){
		countDownText = _countDownText;
		countDownGage = _countDownGage;
		countDownTimeLength = _countDownTimeLength;
	}
		
	void Update () {
	
	}
}
```

## Ejemplo ResourceAsync

```csharp

[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
private static void OnAfterSceneLoadRuntimeMethod() {
	Debug.Log ("After scene loaded - Called Awake and run Configure ");
	ResourceAsync
		.Execute (context)
		.Then(()=>{
			Debug.Load("ResourceAsync OK!")
		}).Catch(()=>{
			Debug.Load("ResourceAsync KO!")
			System.Exit(0);
		});
}
	
```


## Ejemplo promises
```csharp
public void ResolvePromise() {
   var promise = Promise<String>.Resolved("Victor Bolinches");
   var completed = 0;
   promise.Then(v => { ++completed; });
}

public void RejectedPromise() {
   var ex = new Exception();
   var promise = Promise<int>.Rejected(ex);
   var errors = 0;
   promise.Catch(e => { ++errors; );
}

public void ResolvePromiseAndTriggerHandler() {
   var promise = Promise<String>();
   var completed = 0;
   promise.Then(v => { ++completed; });
   promise.Resolved("Victor Bolinches");
}

public void ResolvePromiseAndTriggeMultipleBuilderHandler() {
   var promise = Promise<int>();
   var completed = 0;
   promise
   	.Then(v => { ++completed; })
   	.Then(v => { ++completed; });

   promise.Resolved(3);
}

public void ResolvePromiseAndTriggeMultipleBuilderHandlerGeneric() {
   var promise = Promise<int>();
   var completed = "3";
   promise
   	.Then<string>(v => { chainedPromise; })
   	.Then(v => { Assert.Equal(completed, v); });

   promise.Resolved(3);
}

public void RejectedPromiseAndTriggerHandler() {
   var ex = new Exception();
   var promise = Promise<int>();
   var errors = 0;
   promise.Catch(e => { ++errors; );
   promise.Rejected(ex);
}

public void RejectedPromiseAndTriggeMultipleBuilderHandler() {
   var promise = Promise<int>();
   var completed = 0;
   promise
   	.Catch(v => { ++completed; })
   	.Catch(v => { ++completed; });

   promise.Rejected(3);
}

public void AllPromises() {
  var promise = new Promise<string>();
  var chainedPromise1 = new Promise<int>();
  var chainedPromise2 = new Promise<int>();
  
  var completed = 0;

promise
  .All(i => new List<IPromise<int>>() { chainedPromise1,chainedPromise2 } )
  .Then(result => {
     var items = result.ToArray();
     ++completed;
   });

promise.Resolve("hello");

chainedPromise2.Resolve(10);
chainedPromise1.Resolve(2);
}

public void AllDirectAndRejectedPromises() {
  var chainedPromise1 = new Promise<int>();
  var chainedPromise2 = new Promise<int>();
  
  var completed = 0;

Promise<int>
   .All(i => new List<IPromise<int>>() { chainedPromise1,chainedPromise2 } )
   .Then(result => {
	   throw new ApplicationException("Shouldn't happen");
   })
   .Catch(e => {
      ++errors;
   });
   
chainedPromise1.Reject(new ApplicationException("Error!"));	

chainedPromise2.Resolve(10);
 //**** chainedPromise1.Resolve(2);
}

public void OncePromises() {
  var promise = new Promise<string>();
  var chainedPromise1 = new Promise<int>();
  var chainedPromise2 = new Promise<int>();
  
  var completed = 0;

promise
  .Once(i => new List<IPromise<int>>() { chainedPromise1,chainedPromise2 } )
  .Then(result => {
     var items = result.ToArray();
     ++completed;
   });

promise.Resolve("hello");

chainedPromise2.Resolve(10);
}

public void AllDirectAndRejectedPromises() {
  var chainedPromise1 = new Promise<int>();
  var chainedPromise2 = new Promise<int>();
  
  var completed = 0;

Promise<int>
   .Once(i => new List<IPromise<int>>() { chainedPromise1,chainedPromise2 } )
   .Then(result => {
     var items = result.ToArray();
     ++completed;
   })
   .Catch(e => {
      ++errors;
   });
   
chainedPromise1.Reject(new ApplicationException("Error!"));	
chainedPromise2.Resolve(10);
}

public void DoneWithResolvedReject() {
  var promise = new Promise<int>();
  var callback = 0;
  var errorCallback = 0;
  var expectedValue = 5;

  promise.Done(
    value => { ++callback; },
    ex => { ++errorCallback; }
  );

  promise.Resolve(expectedValue);
}

```


# Errores Comunes

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

* Si queremos lanzar un ICommandAsync debemos de ejecutar la llamada a través de la instancia de la clase del Evento. Ésta se injectará en el comando automáticamente.

El método Execute de la clase comando debe de ser sustituido por ExecuteAsync y devolver una promise.
La clase del comando debe de implementar ICommandAsync.
El dispatcher debe de hacer uso del método ExecuteAsync.

```csharp

eventCommandMap.Map(PuzzleGameConfigureEvent.Type.POST_INITIALIZE_EVENT).ToCommandAsync<PuzzleGameConfigureCommandAsync>();

public PuzzleGameConfigureCommandAsync : ICommandAsync {

	public IPromise ExecuteAsync() {
		var promise = new Promise();
		
		{ ..... }
		
		return promise;
	}
}

dispatcher
	.ExecuteAsync(PuzzleGameConfigureEvent.Create())
	.Then(()=>{
	Debug.Log("Victor Bolinches Marin");
});;

``` 

