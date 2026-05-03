# Glosario de metricas para analitica de futbol profesional

Este documento define las principales metricas usadas por el asistente experto en analitica de futbol profesional. Su objetivo es ayudar al sistema RAG a interpretar correctamente las columnas del dataset `players_data-2024_2025.csv` y a responder preguntas de scouting, rendimiento y comparacion de jugadores.

## Identificacion del jugador

### Player

Nombre del jugador.

Uso analitico: permite buscar, comparar y generar informes individuales.

### Nation

Nacionalidad del jugador.

Uso analitico: puede ser relevante para restricciones de plantilla, scouting internacional o analisis por pais.

### Pos

Posicion principal o combinada del jugador.

Ejemplos habituales:

- `GK`: portero.
- `DF`: defensa.
- `MF`: centrocampista.
- `FW`: delantero.
- `DF,MF`: jugador con uso defensivo y de mediocampo.
- `FW,MF`: atacante con participacion como delantero y mediapunta/extremo.

Uso analitico: las metricas deben interpretarse segun la posicion. No se debe comparar directamente un central con un delantero usando solo goles o xG.

### Squad

Equipo del jugador durante la temporada.

Uso analitico: permite filtrar por club y analizar perfiles dentro de un contexto tactico.

### Comp

Competicion o liga en la que juega el futbolista.

Ejemplos:

- Premier League.
- La Liga.
- Serie A.
- Bundesliga.
- Ligue 1.

Uso analitico: las comparaciones entre ligas deben hacerse con cautela, porque el nivel, ritmo y estilo de juego pueden variar.

### Age

Edad del jugador.

Uso analitico: clave para scouting. Un jugador joven con buenos indicadores por 90 puede tener mayor potencial de desarrollo.

### Born

Año de nacimiento.

Uso analitico: alternativa para calcular edad o segmentar generaciones.

## Tiempo de juego

### MP

Partidos jugados.

Uso analitico: indica participacion, pero no siempre refleja volumen real porque un jugador puede disputar muchos partidos con pocos minutos.

### Starts

Partidos como titular.

Uso analitico: mide confianza del entrenador y rol dentro del equipo.

### Min

Minutos jugados.

Uso analitico: es una de las variables mas importantes para filtrar muestras fiables. Para rankings, conviene exigir un minimo de minutos.

### 90s

Minutos expresados como partidos completos de 90 minutos.

Formula aproximada:

```text
90s = Min / 90
```

Uso analitico: permite normalizar metricas por 90 minutos y comparar jugadores con distinto volumen de juego.

Buena practica: para comparar rendimiento, filtrar jugadores con al menos 900 minutos o `90s >= 10`.

### Min%

Porcentaje de minutos disponibles jugados por el futbolista.

Uso analitico: ayuda a identificar titulares habituales, rotaciones o suplentes.

### Mn/MP

Minutos promedio por partido jugado.

Uso analitico: diferencia jugadores titulares de jugadores usados como revulsivos.

### Mn/Start

Minutos promedio en partidos como titular.

Uso analitico: mide continuidad cuando el jugador empieza el partido.

### Subs

Partidos entrando como suplente.

Uso analitico: util para detectar jugadores de impacto desde el banquillo.

### Mn/Sub

Minutos promedio cuando entra como suplente.

Uso analitico: contextualiza la produccion de jugadores con pocos minutos.

## Produccion ofensiva

### Gls

Goles marcados.

Uso analitico: medida basica de produccion ofensiva. Debe interpretarse junto con minutos, xG y posicion.

### Ast

Asistencias.

Uso analitico: mide contribucion directa al gol, aunque depende mucho de la finalizacion de los companeros.

### G+A

Goles mas asistencias.

Uso analitico: resume participacion directa en goles.

### G-PK

Goles sin penaltis.

Uso analitico: mejor metrica que goles totales para evaluar finalizacion en juego abierto.

### PK

Penaltis marcados.

Uso analitico: separa produccion desde el punto de penalti de la produccion en juego.

### PKatt

Penaltis intentados.

Uso analitico: permite calcular eficacia en penaltis.

### G+A-PK

Goles mas asistencias excluyendo penaltis.

Uso analitico: metrica util para medir impacto ofensivo sin inflar el rendimiento con penaltis.

## Goles esperados y creatividad esperada

### xG

Expected Goals o goles esperados.

Representa la probabilidad acumulada de que los tiros de un jugador terminen en gol segun la calidad de las ocasiones.

Interpretacion:

- xG alto: el jugador recibe o genera ocasiones claras de remate.
- Goles mayores que xG: el jugador ha finalizado por encima de lo esperado.
- Goles menores que xG: el jugador ha finalizado por debajo de lo esperado.

Uso analitico: clave para evaluar delanteros, extremos y mediapuntas.

### npxG

Non-penalty expected goals o goles esperados sin penaltis.

Uso analitico: mejor que xG total para comparar capacidad de generar ocasiones en juego abierto.

### xAG

Expected Assisted Goals.

Mide la calidad de las ocasiones que un jugador crea para sus companeros mediante pases que terminan en tiro.

Uso analitico: clave para evaluar creadores, mediapuntas, interiores y extremos.

### xA

Expected Assists.

Mide la probabilidad de que un pase acabe siendo asistencia, segun la calidad de la ocasion generada.

Uso analitico: evalua creatividad mas alla de las asistencias reales.

### npxG+xAG

Suma de goles esperados sin penaltis y goles asistidos esperados.

Uso analitico: indicador combinado de amenaza ofensiva y creatividad.

### xG+xAG

Suma de xG y xAG.

Uso analitico: medida global de contribucion ofensiva esperada.

### G-xG

Diferencia entre goles reales y goles esperados.

Interpretacion:

- Valor positivo: finalizacion por encima de lo esperado.
- Valor negativo: finalizacion por debajo de lo esperado.

Uso analitico: ayuda a detectar sobre-rendimiento o mala racha de finalizacion.

### np:G-xG

Diferencia entre goles sin penaltis y npxG.

Uso analitico: version mas limpia para evaluar finalizacion sin influencia de penaltis.

### A-xAG

Diferencia entre asistencias reales y xAG.

Interpretacion:

- Valor positivo: los companeros finalizaron muy bien sus pases.
- Valor negativo: sus pases generaron buenas ocasiones, pero no se convirtieron en gol.

Uso analitico: evita juzgar la creatividad solo por asistencias.

## Tiro

### Sh

Tiros totales.

Uso analitico: mide volumen de finalizacion.

### SoT

Tiros a puerta.

Uso analitico: indica precision y amenaza directa.

### SoT%

Porcentaje de tiros que van a puerta.

Uso analitico: mide precision de tiro.

### Sh/90

Tiros por 90 minutos.

Uso analitico: permite comparar volumen de tiro entre jugadores con distintos minutos.

### SoT/90

Tiros a puerta por 90 minutos.

Uso analitico: buen indicador de amenaza ofensiva constante.

### G/Sh

Goles por tiro.

Uso analitico: mide eficiencia de finalizacion.

### G/SoT

Goles por tiro a puerta.

Uso analitico: mide conversion de tiros que obligan al portero a intervenir.

### Dist

Distancia media de los tiros.

Uso analitico: una distancia menor suele indicar tiros de mayor calidad; una distancia alta puede indicar tiros lejanos o baja seleccion de tiro.

### npxG/Sh

xG sin penaltis por tiro.

Uso analitico: mide calidad media de las ocasiones. Es muy util para diferenciar volumen de tiro y calidad de tiro.

## Pase y progresion

### Cmp

Pases completados.

Uso analitico: mide participacion en circulacion.

### Att

Pases intentados.

Uso analitico: indica volumen de pase.

### Cmp%

Porcentaje de pases completados.

Uso analitico: mide seguridad en pase, pero debe interpretarse con el tipo de pase. Un central puede tener Cmp% alto por pases sencillos; un mediapunta puede arriesgar mas.

### TotDist

Distancia total recorrida por los pases completados.

Uso analitico: mide volumen de distribucion.

### PrgDist

Distancia progresiva total de los pases.

Uso analitico: indica capacidad para avanzar el balon hacia zonas peligrosas.

### KP

Key Passes o pases clave.

Pases que terminan en tiro de un companero.

Uso analitico: indicador directo de creacion de ocasiones.

### 1/3

Pases hacia el ultimo tercio del campo.

Uso analitico: mide capacidad de progresar el juego hacia zonas ofensivas.

### PPA

Pases al area rival.

Uso analitico: relevante para interiores, mediapuntas, laterales ofensivos y extremos.

### CrsPA

Centros al area rival.

Uso analitico: util para evaluar laterales y extremos que generan peligro mediante centros.

### PrgP

Pases progresivos.

Uso analitico: una de las mejores metricas para detectar jugadores que hacen avanzar la posesion.

### TB

Through Balls o pases al espacio.

Uso analitico: indica vision, creatividad y capacidad para romper lineas defensivas.

### Sw

Switches o cambios de orientacion.

Uso analitico: util para mediocentros, centrales y laterales con buena distribucion.

### Crs

Centros.

Uso analitico: mide frecuencia de envio lateral al area.

## Creacion de tiros y goles

### SCA

Shot-Creating Actions o acciones que crean tiros.

Incluye acciones ofensivas como pases, regates, faltas recibidas o recuperaciones que terminan en un tiro.

Uso analitico: mide participacion en la creacion de ocasiones.

### SCA90

Acciones creadoras de tiro por 90 minutos.

Uso analitico: permite comparar creatividad entre jugadores con distinto volumen de minutos.

### GCA

Goal-Creating Actions o acciones que crean goles.

Uso analitico: similar a SCA, pero solo para acciones que terminan en gol.

### GCA90

Acciones creadoras de gol por 90 minutos.

Uso analitico: indicador de impacto directo en goles, aunque puede ser mas volatil que SCA90.

### PassLive

Acciones creadoras procedentes de pases en juego.

Uso analitico: mide creatividad en jugada.

### PassDead

Acciones creadoras procedentes de balon parado.

Uso analitico: importante para especialistas en faltas, corners y centros a balon parado.

### TO

Take-ons o regates que participan en una accion creadora.

Uso analitico: relevante para extremos, mediapuntas y laterales ofensivos.

## Defensa

### Tkl

Entradas realizadas.

Uso analitico: mide actividad defensiva, aunque muchas entradas no siempre significan mejor defensa.

### TklW

Entradas ganadas.

Uso analitico: mide eficacia defensiva en duelos de entrada.

### Def 3rd

Acciones defensivas en el tercio defensivo.

Uso analitico: relevante para defensas y mediocentros.

### Mid 3rd

Acciones defensivas en el tercio medio.

Uso analitico: util para evaluar presion y recuperacion en mediocampo.

### Att 3rd

Acciones defensivas en el tercio ofensivo.

Uso analitico: clave para equipos de presion alta y delanteros con trabajo defensivo.

### Tkl%

Porcentaje de entradas exitosas.

Uso analitico: mide eficacia en el duelo defensivo.

### Lost

Duelos o entradas perdidas, segun el contexto de la columna.

Uso analitico: debe interpretarse junto con volumen defensivo.

### Blocks

Bloqueos.

Uso analitico: mide intervenciones para cortar tiros o pases.

### Int

Intercepciones.

Uso analitico: indica lectura de juego, anticipacion y posicionamiento.

### Tkl+Int

Suma de entradas e intercepciones.

Uso analitico: indicador agregado de actividad defensiva.

### Clr

Despejes.

Uso analitico: relevante para defensas sometidos a presion.

### Err

Errores que terminan en ocasion o tiro rival.

Uso analitico: muy importante para defensas y porteros; valores altos son senal de riesgo.

## Posesion y conduccion

### Touches

Toques de balon.

Uso analitico: mide participacion en el juego.

### Def Pen

Toques en el area propia.

Uso analitico: relevante para porteros, centrales y defensas.

### Att Pen

Toques en el area rival.

Uso analitico: mide presencia en zonas de remate.

### Carries

Conducciones con balon.

Uso analitico: mide volumen de transporte de balon.

### PrgC

Conducciones progresivas.

Uso analitico: clave para detectar jugadores que rompen lineas conduciendo.

### CPA

Conducciones hacia el area rival.

Uso analitico: muy relevante para extremos, laterales ofensivos e interiores.

### Succ

Regates exitosos.

Uso analitico: mide capacidad de superar rivales.

### Succ%

Porcentaje de regates exitosos.

Uso analitico: evalua eficacia en el uno contra uno.

### Tkld

Veces que el jugador fue derribado o frenado al intentar regatear.

Uso analitico: ayuda a medir riesgo en conduccion.

### Mis

Controles fallidos.

Uso analitico: indica perdidas por mala recepcion o control.

### Dis

Veces que el jugador fue desposeido.

Uso analitico: mide perdidas bajo presion.

### Rec

Pases recibidos.

Uso analitico: mide involucracion como receptor.

### PrgR

Pases progresivos recibidos.

Uso analitico: clave para delanteros, extremos y mediapuntas que reciben entre lineas o atacan espacios.

## Disciplina y acciones varias

### CrdY

Tarjetas amarillas.

Uso analitico: mide riesgo disciplinario.

### CrdR

Tarjetas rojas.

Uso analitico: indicador de riesgo alto o acciones graves.

### Fls

Faltas cometidas.

Uso analitico: mide agresividad defensiva y riesgo de conceder acciones a balon parado.

### Fld

Faltas recibidas.

Uso analitico: relevante para jugadores que atraen contactos y generan ventajas.

### Off

Fueras de juego.

Uso analitico: relevante para delanteros que atacan la espalda de la defensa.

### Recov

Recuperaciones de balon.

Uso analitico: mide capacidad para recuperar posesion.

### Won

Duelos aereos ganados.

Uso analitico: importante para centrales, delanteros referencia y mediocentros fisicos.

### Won%

Porcentaje de duelos aereos ganados.

Uso analitico: mide eficacia aerea.

### OG

Goles en propia puerta.

Uso analitico: evento puntual, no debe sobreinterpretarse sin contexto.

## Metricas de porteros

### GA

Goles encajados.

Uso analitico: debe interpretarse con tiros recibidos, calidad de tiros y contexto defensivo del equipo.

### GA90

Goles encajados por 90 minutos.

Uso analitico: permite comparar porteros con distinto volumen de minutos.

### SoTA

Tiros a puerta recibidos.

Uso analitico: mide carga de trabajo del portero.

### Saves

Paradas realizadas.

Uso analitico: mide volumen de intervenciones.

### Save%

Porcentaje de paradas.

Uso analitico: indicador basico de rendimiento, pero no considera calidad de los tiros.

### CS

Clean Sheets o porterias a cero.

Uso analitico: depende tanto del portero como del sistema defensivo.

### CS%

Porcentaje de partidos con porteria a cero.

Uso analitico: comparacion de solidez defensiva, con cautela por contexto de equipo.

### PSxG

Post-Shot Expected Goals.

Mide la calidad de los tiros a puerta recibidos despues de considerar colocacion y caracteristicas del disparo.

Uso analitico: muy util para evaluar porteros.

### PSxG+/-

Diferencia entre PSxG recibido y goles encajados.

Interpretacion:

- Valor positivo: el portero evita mas goles de lo esperado.
- Valor negativo: encaja mas goles de lo esperado.

Uso analitico: una de las mejores metricas para evaluar shot-stopping.

### PSxG/SoT

Calidad media de los tiros a puerta recibidos.

Uso analitico: contextualiza la dificultad de las paradas.

### #OPA

Acciones defensivas del portero fuera del area.

Uso analitico: mide comportamiento como portero-limbero.

### #OPA/90

Acciones defensivas fuera del area por 90 minutos.

Uso analitico: permite comparar agresividad y altura del portero en la defensa del espacio.

### AvgDist

Distancia media desde la porteria en acciones defensivas del portero.

Uso analitico: indica si el portero participa lejos de su porteria.

## Recomendaciones de uso para scouting

### Delanteros centro

Metricas recomendadas:

- `xG`
- `npxG`
- `Gls`
- `G-PK`
- `Sh/90`
- `npxG/Sh`
- `SoT/90`
- `PrgR`
- `Att Pen`

Interpretacion: un buen delantero suele combinar volumen de ocasiones, calidad de tiro, presencia en area y capacidad para recibir pases progresivos.

### Extremos

Metricas recomendadas:

- `xG`
- `xAG`
- `SCA90`
- `PrgC`
- `CPA`
- `Succ`
- `Succ%`
- `Crs`
- `KP`
- `PPA`

Interpretacion: un extremo debe aportar amenaza, desborde, progresion y creacion de ocasiones.

### Mediapuntas e interiores ofensivos

Metricas recomendadas:

- `xAG`
- `xA`
- `KP`
- `SCA90`
- `GCA90`
- `PrgP`
- `PPA`
- `1/3`
- `TB`

Interpretacion: se prioriza creatividad, ultimo pase, progresion y participacion en acciones de tiro.

### Mediocentros

Metricas recomendadas:

- `Cmp`
- `Att`
- `Cmp%`
- `PrgP`
- `PrgDist`
- `1/3`
- `Tkl+Int`
- `Recov`
- `Touches`

Interpretacion: un mediocentro debe combinar seguridad, progresion, volumen de participacion y trabajo defensivo.

### Laterales

Metricas recomendadas:

- `PrgC`
- `PrgP`
- `Crs`
- `CrsPA`
- `PPA`
- `Touches`
- `Tkl`
- `Int`
- `Blocks`
- `SCA90`

Interpretacion: un lateral moderno debe aportar salida, profundidad, centros y solidez defensiva.

### Centrales

Metricas recomendadas:

- `Tkl`
- `Int`
- `Clr`
- `Blocks`
- `Err`
- `Cmp%`
- `PrgP`
- `PrgDist`
- `Won%`

Interpretacion: un central debe ser fiable defensivamente, fuerte en duelos y capaz de progresar el balon cuando el modelo de juego lo requiere.

### Porteros

Metricas recomendadas:

- `Save%`
- `PSxG`
- `PSxG+/-`
- `PSxG/SoT`
- `CS%`
- `#OPA/90`
- `AvgDist`
- `Cmp%_stats_keeper_adv`

Interpretacion: un buen analisis de porteros debe separar shot-stopping, juego con pies y control del espacio.

## Buenas practicas de comparacion

1. Comparar jugadores de la misma posicion o rol tactico.
2. Filtrar por minutos minimos antes de crear rankings.
3. Preferir metricas por 90 cuando el tiempo de juego es distinto.
4. Interpretar goles y asistencias junto con xG, npxG, xAG y xA.
5. No sacar conclusiones fuertes con muestras pequenas.
6. Contextualizar por equipo, liga y estilo de juego.
7. Separar produccion real de rendimiento esperado.
8. Para scouting joven, combinar edad, minutos y metricas por 90.

## Preguntas que el agente debe poder responder con este glosario

- Que significa xG y como se interpreta?
- Que diferencia hay entre xG, npxG, xAG y xA?
- Que metricas debo usar para evaluar a un delantero?
- Que metricas son relevantes para analizar un lateral ofensivo?
- Como comparo dos mediocentros usando datos?
- Que indicadores ayudan a identificar un extremo desequilibrante?
- Que metricas sirven para evaluar a un portero moderno?
- Por que no debo comparar jugadores con pocos minutos sin filtrar?
