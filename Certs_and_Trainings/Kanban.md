Libro: "Principles of Product Development Flow"

###### Sistema push: 
En un sistema push, la producción o las tareas se inician y se "empujan" a lo largo del proceso ==basado en pronósticos o planificaciones previas.== Las decisiones sobre qué y cuánto producir se toman anticipadamente y se "empujan" hacia las siguientes etapas del proceso, independientemente de la demanda actual.
- **Ventajas**: Puede ser útil en entornos donde la demanda es constante y predecible.
- **Desventajas**: Puede llevar a la acumulación de inventarios innecesarios, desperdicio de recursos y falta de flexibilidad ante cambios en la demanda.

###### Sistema pull:
En un sistema pull, la producción o las tareas se inician en respuesta a la ==demanda rea==l. Es decir, las tareas solo se realizan cuando hay una solicitud específica o cuando el siguiente paso en el proceso lo requiere.
- **Ventajas**: Minimiza el desperdicio, reduce inventarios y permite una mayor flexibilidad para adaptarse a cambios en la demanda.
- **Desventajas**: Puede ser mas difícil de implementar en entornos con demanda altamente variable o impredecible.

En Kanban (PULL), el trabajo se visualiza en un tablero y las tareas se inician ==solo cuando hay capacidad disponible== y una necesidad real, siguiendo principios de flujo y demanda. Esto asegura que las tareas se realicen de manera eficiente y oportuna, evitando sobreproducción y acumulación de trabajo en proceso.

**Principios del Kanban como Sistema Pull**:

1. **Visualización del trabajo**: Las tareas se representan en un tablero Kanban, proporcionando una vista clara del flujo de trabajo.
2. **Límite del trabajo en progreso (WIP)**: Se establecen límites en la cantidad de trabajo que puede estar en progreso en cada etapa del proceso, evitando la sobrecarga.
3. **Gestión del flujo**: Se monitorea y gestiona el flujo de trabajo para identificar y eliminar cuellos de botella.
4. **Retroalimentación continua**: Se realizan reuniones regulares para revisar y mejorar el proceso, basándose en datos reales y experiencias.

#### las 5 reglas de Kanban

1- Visualizar el flujo (kanban board, a pesar de que no es realmente una parte esencial de kanban)
2- Limitar el WIP (work in progress)
3- Medir y manejar el flujo
4- Hacer las policies (reglas) explicitas
5- Reconocer las oportunidades de mejora (revision periódica) --> kaizen

Concepto de **LEAN**: Eliminar el "residuo" o lo que no aporta valor

Mientras Lean abarca una filosofía y un conjunto de principios para mejorar cualquier tipo de proceso organizacional, Kanban se enfoca específicamente en la gestión del flujo de trabajo mediante un sistema pull visual, complementando y poniendo en práctica muchos de los principios Lean.

- **Enfoque en la Mejora Continua**: Tanto Lean como Kanban promueven la mejora continua (Kaizen) y la optimización de procesos.
- **Eliminación de Desperdicio**: Ambos métodos buscan identificar y eliminar actividades que no agregan valor, conocidas como "muda" en Lean.
- **Valor para el Cliente**: Se centran en maximizar el valor entregado al cliente, asegurando que cada paso en el proceso contribuya a este objetivo.

sería "buscar the leanest path from beginning to end"

##### Kanban Wall ó Dashboard
El dashboard no es el método en sí mismo, sino simplemente un signalling device

Buffer columns: pueden tener sentido dependiendo del flujo de trabajo
![[Pasted image 20240525125953.png]]


se le puede (debe) poner WIP limits a cada columna. 


##### Determinar la capacidad:

Esto es exactamante lo que nos pasa a nosotros:

> *"Your organization will likely place certain demands on your team.*
> 
> *That makes sense. After all, that's why your teams exist.*
> *They help the organization deliver some product or service.*
> *Chances are your organization will have inconsistent demands.*
> *One department might have a lot of demand one week and a different department will demand even more than next.*
> *There's not much your team can do about increasing or decreasing your organization's demands. Instead, you have to focus on your team's capacity.*
> ***Most organizations don't think about your team's capacity when they create a demand.***
> 
> *That's actually why a lot of teams get into trouble. Outside departments try to stuff their work into your team's workflow.* *They often don't realize that overloading the team slows down everyone's work.* ***Even if they did realize that, then they might not care.***
> *They're probably not interested in optimizing the overall organization.*
> 
> *Instead, they're just focused on fixing their immediate challenges.*
> *That's why it's up to the kanban team to be very disciplined in how they determine their capacity.*
> 
> *Remember that demand can come in many different shapes and sizes."*

Para esto se utilizan las "Swimlanes". 
Se deben poder establecer limites por swimnlane. Por ejemplo puede haber una por equipo, y sus peticiones se van encolando. Y otra para "Critical" que realmente sean critical.


-------------


**Ley de Little:**  
La Ley de Little establece que el número promedio de elementos en un sistema (L) es igual a la tasa de llegada de elementos (λ) multiplicada por el tiempo promedio que un elemento pasa en el sistema (W):
L=λ×W

L: Work in progress(WIP), numero promedio de tareas en el sistema
λ: throughput: Cantidad de tareas terminadas en un periodo de tiempo
W: Tiempo promedio que una tarea pasa en el sistema.

W = L / λ. 

--------

El kanban Dashboard solo es una foto del momento actual, pero no te permite saber si a lo largo del tiempo el flujo funciona correctamente.

**CFD**: Cumulative Flow Diagram
![[Pasted image 20240525171516.png]]

Distribucion de las tareas:

Se aconseja reducir el tiempo de estimacion:
- Dividir tareas grandes
- Clases de servicio: (tipos de trabajo)
	- Tareas Urgentes: Con su propio swimlane
	- Tareas standar
	- Tareas de Larga Duracion
	- Tareas de bajo riesgo (e.g deuda tecnica): trabajadas cuando haya capacidad disponible

wdwdwe