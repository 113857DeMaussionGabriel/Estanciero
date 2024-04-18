# TP Grupal: El Estanciero
- **Integrantes**: 
     - 113857 De Maussion, Gabriel
     - 113973 Luparia Mothe, Agustin
     - 113895 Ortiz, Pablo
     - 114320 Ferreyra, Fernando Gabriel
     - 113174 Gregorutti, Stefano
     - 114179 Leticia, Castro
     - 114143 Nadia, Gobetto
## Clases

### 1. **Estanciero**
 - Funciona como clase principal, se encarga de iniciar/cargar las partidas.
 - __Atributos__:
  - `partida` : Un objeto de clase Tablero que contiene la logica de juego que se lleva a cabo dentro de la partida. 
 - __Métodos__:
  - `CrearTablero()` : Consulta si se desea iniciar una nueva partida, en cuyo caso se cargara un tablero en blanco, se agregan jugadores "virtuales" en base a la dificultad y se ordenan los jugadores, si no, se puede cargar una partida ya empezada desde la base de datos.
  - `OrdenarJugadores()` : Ordena la lista de jugadores en el Tablero en base a una tirada de dados inicial de mayor a menor y muestra dicho orden por consola.
  - `SeleccionarDificultad()` : Permite establecer el nivel de dificultad del juego, agregando jugadores virtuales con distintos perfiles en la partida.
  - `CargarTablero()` : Consulta, interpreta y carga los datos de una partida en la base de datos ya empezada para establecerla como la actual.
  - `AgregarJugador()` : Agrega al jugador e instancia los distintos perfiles de jugadores virtuales para sumarlos a la lista de jugadores.


### 2. **Tablero**
   - Contiene la mayor parte de la logica del juego, siendo el equivalente a los datos de la partida.
   - __Atributos__:
        - `jugadores` : Una lista de objeto "Jugador" ordenada en base a los turnos.
        - `casillas` : Una lista de las 42 casillas en las que los jugadores pueden caer en el tablero.
        - `dado` : Una instancia de la clase, para llamar a sus metodos y recuperar el valor de cada dado.
        - `cartasSuerte` : Una lista de las 16 cartas posibles para cuando el jugador tenga que levantar
        - `cartasDestino` : Una lista de las 16 cartas posibles para cuando el jugador tenga que levantar
        - `cantidadGanadora` : Un valor entero que al alcanzar un jugador indicara el fin de la partida.
   - __Métodos__:
        - `Turno()` : Gestiona el turno de cada jugador, decidiendo si puede comprar, mejorar o levantar cartas entre otras.
        - `BalanceJugador()` : Verifica y actualiza el balance de los jugadores, sumando o restando montos.
        - `EliminarJugador()` : Remueve un jugador de la lista en caso de que este en bancarrota.
        - `MostrarEstado()` : Muestra el estado actual del juego.
        - `TerminarPartida()` : Finaliza el juego y determina jugador ganador.
        - `MoverJugador()` : Mueve el jugador por el tablero según el resultado del dado o cartas.
        - `PagarAlquiler()` : Gestiona el pago de alquileres cuando un jugador cae en una propiedad ajena.
        - `LevantarCarta()` : Levanta y usa una carta de la lista para luego enviarla al final del mazo.
        - `MesclarMazo()` : Mezcla la lista de cartas antes de iniciar la partida.


### 3. **Casilla**
   - Es la clase padre con atributos comunes entre los distintos tipos de casillas.
   - __Atributos__:
        - `id` : Identificador de la casilla.
        - `nombre` : Nombre de la casilla.


### 4. **Carta**
   - Clase encargada de almacenar los datos de una carta y mostrar las acciones que se deben realizar al levantarse.
   - __Atributos__:
        - `descripcion` : Cadena de caracteres a modo de mensaje con el efecto de la carta. 
   - __Metodos__:
        - `UsarCarta` : Mostrar por consola la descripcion de la carta y devuelve 2 valores, el efecto de la carta (mover jugador, dar dinero, etc) ademas de la intensidad del mismo (mover 1 o 2 casillas).


### 5. **Especial** (hereda de **Casilla**)
   - **por definir**
   - __Atributos__:
        - `descripcion` : **por definir**
   - __Metodos__:
        - `Efecto` : **por definir**


### 6. **Propiedad** (hereda de **Casilla**)
   - Clase padre para todas aquellos tipos de casillas que sean comprables e imponen un alquiler sobre los jugadores que caigan en ellas.
   - __Atributos__:
        - `tipo` : Una cadena de caracteres (O una ennumeracion) para acelerar la busqueda y filtro entre las variantes de propiedades (Escritura, Ferrocarril y Compañia).
        - `precio` : El importe que se debe abonar para que un jugador adquiera la propiedad de la casilla.
        - `alquileres` : Una lista de valores enteros que varia segun el nivel de la escritura o cantidad de propiedades iguales que posea el dueño de la casilla.
        - `valorHipoteca` : El monto que se le otorgara al jugador si desea hipotecar la propiedad.
        - `hipotecado` : Un booleano para comprobar el estado de la propiedad.
   - __Métodos__:
        - `CalcularAlquiler()` : En base a los valores de alquiler se encarga de devolver un entero con el monto que se le debe cobrar al jugador que caiga en la casilla.
        - `Hipotecar()` : Hipoteca la propiedad.
        - `Deshipotecar()` : Levanta la hipoteca de la propiedad.


### 7. **Escritura** (Hereda de **Propiedad**)
   - Especifica detalles de quellas propiedades "escrituras" comprables (22 en total) que pueden ser mejoradas agregándoles Chacras o Estancias.
   - __Atributos__:
        - `provincia`: String con la ubicación de la propiedad.
        - `zona`: String con la zona específica dentro de la provincia.
        - `nivel`: valor entero que indica el nivel de desarrollo de la propiedad (número de Chacras/Estancias).
   - __Métodos sobreescritos__:
        - `CalcularAlquiler()` : Devuelve un numero entero con el monto que se deberia cobrar al jugador que caiga en la casilla teniendo en cuenta el nivel (cantidad de chacras/estancias) que esten en la casilla.


### 8. **Compañia** (Hereda de **Propiedad**)
   - Sobreescritura para las 3 casillas que utilizan atributos y metodos de la clase padre y sobreescribe aquellos que sean necesarios para el calculo del alquiler.
   - __Métodos sobreescritos__:
        - `CalcularAlquiler()` : Devuelve un numero entero con el monto que se deberia cobrar al jugador que caiga en la casilla teniendo en cuenta la cantidad de compañias que posea el dueño de la casilla haciendo uso  del atributo de `Alquileres` y una tirada de dados.


### 9. **Ferrocarril** (Hereda de **Propiedad**)
   - Detalles para las 4 casillas que utilizan atributos y metodos de la clase padre y sobreescribe aquellos que sean necesarios para el calculo del alquiler.
   - __Métodos sobreescritos__:
        - `CalcularAlquiler()` : Devuelve un numero entero con el monto que se deberia cobrar al jugador que caiga en la casilla teniendo en cuenta la cantidad de ferrocarriles que posea el dueño de la casilla haciendo uso  del atributo de `Alquileres`.


### 10. **Jugador**
   - Clase para el usuario, los atributos ayudan a llevar registro de su progreso y los metodos le habilitan a realizar distintas acciones durante su turno.
   - __Atributos__:
        - `nombre` : Identificacion del jugador/jugador virtual dentro del tablero.
        - `dinero` : Un valor entero que representa el capital del que dispone el jugador.
        - `posicion` : Un numero entero que indica la casilla sobre la que el jugador se encuentra parado en el tablero
        - `propiedades` : Una lista de enteros que hace referencia a las propiedades que el jugador posee de la lista de casillas (se almacena el indice de la casilla en la lista).
   - __Métodos__:
        - `Comprar()`: Compra propiedades.
        - `Vender()`: Vende propiedades.
        - `Mejorar()` : Permite la mejora de las escrituras que posea el jugador agregando chacras o cambiandolas por una estancia.
        - `Hipotecar()`: Hipoteca propiedades.
        - `Deshipotecar()`: Levanta hipotecas.


### 11. **JugadorVirtual** (hereda de **Jugador**)
   - Es una clase abstracta de la que derivan diferentes estrategias de juego (`PerfilAgresivo`, `PerfilConservador`, `PerfilEquilibrado`).
   - __Atributos__:
        - `provinciasPrioritarias` : Una lista de "String"s con los nombres de las provincias que el jugador virtual prioriza al comprar.
   - __Métodos sobreescritos__
        - `Comprar()` : Compra propiedades en base a las preferencias del perfil.
        - `Vender()`: Vende propiedades si llegase a ser necesario.
        - `Mejorar()` : Permite la mejora de las escrituras que posea el jugador agregando chacras o cambiandolas por una estancia basandose en las preferencias del perfil.


### 12. **Dado**
   - Clase encargada de almacenar las tiradas del 1 al 6 para permitir el movimiento de los jugadores entre otras cosas.
   - __Atributos__:
        - `tirada1` : Numero entero del 1 al 6 para el primer dado.
        - `tirada2` : Numero entero del 1 al 6 para el segundo dado.
   - __Metodos__:
        - `TirarDados()` : Devuelve dos numeros aleatorios del 1 al 6  y los asigna en las variables `tirada1` y `tirada2`.

