# Flutter-Login-Page-UI-Explained

Este pequeño tutorial es una explicación del video del desarrollador llamado Abdi Hamid, todo el código es propiedad del desarrollador, lo único que estoy haciendo es explicar algunas de las partes para que cualquier persona que esté iniciando en Flutter lo pueda entender. Ante todo gracias a [Abdi Hamid](https://github.com/devefy) por compartir el video y el código en su repositorio. 

El resultado final con este ejemplo es el siguiente:

![](Logo.png)

En la carpeta lib del proyecto vamos a encontrar cuatro archivos, estos son los que crea Abdi en el video y son los que componene toda el login de la App, estos son:

* main.dart
* CustomIcons.dart
* SocialIcons.dart
* FormCard.dart

En Flutter es muy sencillo ver como se desarrolla toda la composición de la app, simplemente viendo cada Widget y como esta contenido un Widget en otro, esto permite facilmente poder leer y luego interpretar que esta haciendo cada Widget.

Inicamos con el StatefulWidget.

```dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => new _MyAppState();
}

class _MyAppState extends State<MyApp> {
  bool _isSelected = false;

  void _radio() {
    setState(() {
      _isSelected = !_isSelected;
    });
  }
```
Un widget que tiene un estado mutable. State es información que se puede leer de forma sincrónicamente y puede cambiar durante el tiempo de vida del widget, es responsabilidad del implementador asegurar que el State es notificado cuando se genera un cambio, por medio del State.setState.

El Stateful widget es muy util cuando parte de la interfaz de usuario que estamos creando cambia dinamicamente, esta es la diferencia entre el Stateful Widget y el Stateless Widget.

Podemos ver que en el código anterior se usa el Stateful y luego se define el State que más adelante vamos a usar para notificar en los cambios realizados en el Login, por ejemplo cuando se activa el radioButton para recordar los datos de la persona que se va a loggear, en estas situaciones donde el usuario interactua con la app, que debemos hacer uso del StatefulWidget y notificar por medio del State.setState, si hacemos todo esto pero usando el Stateless, no se va dar el cambio en el radioButton.

En este punto puede surgir la pregunta sobre por que el Stateful y el State se definen de forma separada? Los objetos State son de larga vida, pero el StatefullWidget se desecha y se reconstruyen cada vez que cambia la configuración. Como el State no se pierde en cada rebuild, evita cálculos costosos, y obtiene propiedades del State, getters, setters, etc. cada vez que se reconstruye frame a frame.

Lo importante es que esto es lo que permite que existan animaciones en Flutter. Como el State no se desecha, se puede reconstruir constantemente su Widget en respuesta a los cambios en los datos, y cuando sea necesario, si es necesario.

![](radioButton.gif)

En la siguiente sección se define el Widget radioButton, su tamaño, ubicación, la forma y el color, en el child se usa un operador ternario para reemplazar el if y hacer el código más organizado, es facíl entender este operador si seguimos la siguiente relación WTF, en la cual W es la pregunta el que?, luego la T es el True, lo que se implementa si la condición es verdadera y luego la F es el false, lo que se implementa si no se cumple, en este caso el isSelected es la pregunta o la parte W, despues del signo de interrogación ?, luego despues de los dos puntos : vamos a encontrar la parte F del operador ternario.


```dart
Widget radioButton(bool isSelected) => Container(
        width: 16.0,
        height: 16.0,
        padding: EdgeInsets.all(2.0),
        decoration: BoxDecoration(
            shape: BoxShape.circle,
            border: Border.all(width: 2.0, color: Colors.black)),
        child: isSelected // W
            ? Container( // T
                width: double.infinity,
                height: double.infinity,
                decoration:
                    BoxDecoration(shape: BoxShape.circle, color: Colors.black),
              )
            : Container(), // F
      );
```
Después de definir el radioButton, vamos a iniciar con uno de los bloque de código más grandes de toda nuestra app donde se define la mayoria de la estructura del proyecto el Widget build, que hace parte de todo el ciclo de vida del Stateful Widget, Este método se llama a menudo (fps + render). Es obligatorio, hacer un @override y debe devolver un Widget.

Podemos ver como inicia con el @override y al final como retorna un Widget, en este caso un Scaffold.

```dart
@override
  Widget build(BuildContext context) {
    ScreenUtil.instance = ScreenUtil.getInstance()..init(context);
    ScreenUtil.instance =
        ScreenUtil(width: 750, height: 1334, allowFontScaling: true);
    return new Scaffold
 ```
Pero acá hay un package que se esta utilizando y es importante saber que hace, el ScreenUtil. Un plugin para Flutter para adaptar la pantalla y el tamaño de fuente. ¡Deje que su interfaz de usuario muestre un diseño razonable en diferentes tamaños de pantalla!

Para instalar este plugin en Flutter debemos agregar la siguiente línea en el archivo pubsec.yaml y esto instalara las dependencias necesarias para poder utilizarlo.

* flutter_screenutil: ^0.4.2

En el siguiente link podemos encontrar más información al respecto: [ScreenUtil](https://github.com/OpenFlutter/flutter_screenutil)

Nuestro build retorna un Scaffold, veamos una breve explicación de este y cada uno de los Widgets que contiene:

**Scaffold:** Implementa la estructura básica de Material Design. Esta clase proporciona la API para mostrar drawers, Snack Bars y bottom sheets.

**Stack:** Un widget que coloca a sus children en relación con los bordes de su caja. Esta clase es útil si desea superponer a varios child de una manera sencilla, por ejemplo, con algo de texto y una imagen, superpuesta con un degradado y un botón adjunto en la parte inferior.

Podemos ver como luego del Stack, se definen los children:

```dart
Stack(
        fit: StackFit.expand,
        children: <Widget>[
          Column(
            crossAxisAlignment: CrossAxisAlignment.end,
            children: <Widget>[
 ```
 **Padding:** El padding permite generar un espacio entre la ubicación del contenido y el borde de la caja, es más fácil identificarlos cuando cambiamos el valor de 20 a 50. La imagen izquierda tiene un padding de 20 y la derecha de 50.
 
 ![](Padding20n.png)  ![](Padding50n.png)
 
 **Expanded:** Un widget que expande un elemento secundario de una Fila, Columna o Flex para que el child llene el espacio disponible.
 
 **SingleChildScrollView:** Un cuadro en el que se puede desplazar un solo widget.
 
 **SizedBox:** El SizedBox es una caja con un tamaño especifico, pero en este caso podemos ver que se utiliza el ScreenUtil para generar un espacio en el cual se va a poner la FormCard, que es en la cual esta el Login.
 
 **FormCard:** Para esta podemos ver que se creo otro código separado, esto es muy comun cuando nuestro código esta muy grande, de esta forma es más organizado y nos facilita encontrar errores. En la siguiente imagen podemos ver la FormCard.
 
 ![](FormCard.png)
 
En la FormCard se usa un **BoxDecoration**, que permite crear esta caja blanca con bordes redondeados y una sombre en la parte superior e inferior que da la sensación de relieve, esto se consigue usando los dos **BoxShadow** para poder usar esta FormCard en el main.dart debemos importarla, podemos ver en la parte superior del main.dart el siguiente línea de código:
 
```dart
  import 'Widgets/FormCard.dart';
```

Despues del FormCard(), se crea un SizedBox para generar un espacio entre la FormCard y el nuevo Widget que en este caso es un Row, que va contener el radioButton, el Text del "Remember me" y el botón de Sing In.

![](Row.png)

Para el Botón se utiliza un InkWell que es un Widget de área rectangular que responde al touch, luego tenemos un BoxDecoration es una clase que nos da una variedad de formas diferentes sobre como dibujar una caja, tiene un borde, un cuerpo y una sombra, el cuerpo de la caja esta pintada en diferentes layers, el color que llena la caja, luego un gradiente que también llena la caja, cuenta con un boxShadow similar al que vimos anteriomente en el FormCard.

Luego tenemos otro SizedBox para separar el Row anterior del nuevo row que va contener dos líneas horizontales y el Text del Social Login, este Row contiene dos horizontalLine() que se definieron en la parte superior con el siguiente código:

```dart
 Widget horizontalLine() => Padding(
        padding: EdgeInsets.symmetric(horizontal: 16.0),
        child: Container(
          width: ScreenUtil.getInstance().setWidth(120),
          height: 1.0,
          color: Colors.black26.withOpacity(.2),
        ),
      );
 ```
Luego tenemos otro SizedBox para separar el Row anterior de la sección de botones para hacer el login, en este caso se utiliza otro Row pero hace referencia a otra clase llamada SocialIcon que contiene la definición del Widget, esta clase contiene un BoxDecoration de forma circular, un LinearGradient y como child un RawMaterialButton.

Código del Row:

```dart
Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: <Widget>[
                      SocialIcon(
                        colors: [
                          Color(0xFF102397),
                          Color(0xFF187adf),
                          Color(0xFF00eaf8),
                        ],
                        iconData: CustomIcons.facebook,
                        onPressed: () {},
                      ),
                      SocialIcon(
                        colors: [
                          Color(0xFFff4f38),
                          Color(0xFFff355d),
                        ],
                        iconData: CustomIcons.googlePlus,
                        onPressed: () {},
                      ),
                      SocialIcon(
                        colors: [
                          Color(0xFF17ead9),
                          Color(0xFF6078ea),
                        ],
                        iconData: CustomIcons.twitter,
                        onPressed: () {},
                      ),
                      SocialIcon(
                        colors: [
                          Color(0xFF00c6fb),
                          Color(0xFF005bea),
                        ],
                        iconData: CustomIcons.linkedin,
                        onPressed: () {},
                      )
                    ],
                  ),
 ```
 Código de la clase **SocialIcons** se definen tres propiedades, colors, iconData, onPressed, luego tenemos un constructor que recibe estos parametros para poder construir los botones, si vemos en el código anterior se utiliza el iconData, pero este pasa un CustomIcons el cual esta definido en otra clase para poder ser utilizado.
 
 ```dart
 class SocialIcon extends StatelessWidget {
  final List<Color> colors;
  final IconData iconData;
  final Function onPressed;
  SocialIcon({this.colors, this.iconData, this.onPressed});
  @override
  Widget build(BuildContext context) {
    return new Padding(
      padding: EdgeInsets.only(left: 14.0),
      child: Container(
        width: 45.0,
        height: 45.0,
        decoration: BoxDecoration(
            shape: BoxShape.circle,
            gradient: LinearGradient(colors: colors, tileMode: TileMode.clamp)),
        child: RawMaterialButton(
          shape: CircleBorder(),
          onPressed: onPressed,
          child: Icon(iconData, color: Colors.white),
        ),
      ),
    );
  }
}
```
En el Row cuando se utiliza la clase SocialIcon al final se utiliza el iconData que entrega el CustomIcons definido en otra clase, su código es el siguiente:

```dart
class CustomIcons {
  static const IconData twitter = IconData(0xe900, fontFamily: "CustomIcons");
  static const IconData facebook = IconData(0xe901, fontFamily: "CustomIcons");
  static const IconData googlePlus =
      IconData(0xe902, fontFamily: "CustomIcons");
  static const IconData linkedin = IconData(0xe903, fontFamily: "CustomIcons");
}
```
Podemos ver como se van creando diferentes clases que contienen Widgets que en este ejemplo son repetitivos, como es el caso de los botones del login en la parte inferior, es común cuando encontramos en nuestro diseño un Widget que se va a utilizar varias veces, separarlo en una nueva clase y luego simplemente lo llamamos, entregando los valores necesarios, así evitamos saturar nuestro código principal, generar un orden de trabajo, ahorramos tiempo y facilitamos el desarrollo en general.
 
## Creditos

**Código** [Abdi Hamid](https://github.com/devefy)
**Diseño** [Nithinraj Shetty](https://www.uplabs.com/posts/login-99a29cbb-2952-4550-a977-5081bada091d)
