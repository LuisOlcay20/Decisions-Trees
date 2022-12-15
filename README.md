# Decisions Trees
 Se aplica un decision tree/ bagging tree/ random forest para predecir un accidente cerebrovascular.
 
 A partir del primer árbol expuesto (sin podar), se pueden inferir las siguientes conclusiones:
• La variable más importante a la hora de predecir un stroke es la edad.
• Una persona que tiene menos de 57 años es poco probable que sufra un stroke.
• Si una persona tiene más de 75 años, el índice de masa corporal tiene mayor 
importancia que el promedio de glucosa en la sangre para predecir un stroke.
• Si una persona tiene menos de 76 años, el promedio de glucosa en la sangre tiene 
mayor importancia que el índice de masa corporal para predecir un stroke.
Por otra parte, podemos obsevar que el modelo tiene un accuracy de 0.933, lo que quiere 
decir que logra clasificar correctamente un 93,33% e incorrectamente un 6,7% de las 
predicciones.

Observamos que los valores óptimos de los hiperparámetros son:
• cost_complexity= 1e-10
• tree_depth = 8

Teniendo en consideración que el árbol podado posee los siguientes hiperparámetros:
• cost_complexity= 1e-10
• tree_depth = 8
• min_n = 15 (mismo que el árbol original)

Sabemos que el cost_complexity es un método de penalización (Loss Penalty). El grado de 
penalización se encuentra por medio del tunning del hiperparámetro alfa. Cuando alfa = 0, 
no existe penalización y el árbol podado es parecido al original. A medida que aumenta alfa, 
la penalización es mayor y los árboles podados son más pequeños que los originales. Por 
consiguiente, al tener un alfa cercano a 0, la penalización es muy baja. Por otra parte, el 
tree_depth baja de 20 a 8,lo que significa que el árbol ahora puede tener un máxima 
profundidad de 8. Si bien, el cambio en este hiperparámetro es significativo, el árbol 
original ya estaba utilizando una profundidad parecida a pesar de tener la libertad de 
utilizar una profundidad de hasta 20.

Por lo tanto, el árbol podado es estructuralmente igual al árbol orginal, esta condición se 
conoce como correlación entre los árboles y se da cuando el decision tree es un modelo que 
logra percibir correctamente la relación entre los predictores y la respuesta, lo que genera 
una correlación entre los predictores de ambos árboles (correlación entre sus 
predicciones). Por otra parte, el árbol podado tiene menor profundidad, hecho que hace 
que el árbol podado sea más corto que el original. Todo esto se traduce en que el árbol 
podado sea más preciso a la hora de clasificar, obteniendo un acurracy de 93,6% y, como 
consecuencia, clasifique incorrectamente un 6,4% de las predicciones.
Más específicamente, del gráfico del tunning que aparece en f) podemos observar como el 
pequeño aumento de acurracy del árbol podado se debe principalmente a una disminución 
del tree_depth y no del cost_complexity, pues recién a partir de un cost_complexity similiar 
a 1e-3, se comienzan a observar mejoras en el acurracy, pero a costa de una disminución en 
el roc_auc.

De los resultados podemos notar que las variables más importantes para el arbol podado 
son la edad, el promedio de glucosa en la sangre y el índice del masa corporal.

Teniendo en consideración que el modelo de bagging tree posee los siguientes hiperparámetros:
• cost_complexity= 1e-10 (mismo que el árbol podado)
• tree_depth = 8 (mismo que el árbol podado)
• min_n = 21
A través del gráfico podemos observar que por lo general, combinaciones de 
hiperparámetros que contengan un tree_depth bajo nos llevará a obtener altos valores de 
acurracy, pero bajos valores de roc_auc. Por otra parte, podemos observar que la 
combinación de hiperparámetros obtenidos a través del tunning nos ofrece un alto valor de 
roc_auc y un acurracy de 94,2%, lo que significa que el modelo clasifica erróneamente un 
5,8% de las predicciones.
El aumento en acurracy en comparación a los otros 2 árboles se debe gracias a que el 
bootstrap aplicado permite reducir la varianza, siempre y cuando los modelos agregados 
no se correlacionen. Como existe un pequeño aumento en acurracy, podemos inferir que la 
correlación entre los modelos agregados es alta. Si bien al utilizar bagging se mejora la 
predicción de un decision tree, se pierde la interpretabilidad de este. Sin embargo, al 
observar la matriz de confusión nos damos cuenta de que está prediciendo todos los 
valores como 0 y ninguno como 1 (stroke), a diferencia del modelo anterior. Por lo tanto, es 
peor que el modelo anterior.

Debido a que el modelo de Random Forest realiza una selección aleatoria de características, 
los métodos tradicionales de NAs implementados por los decisions trees no aplican. Por 
consiguiente, imputamos los datos a través del comando “rfImpute” que para predictores 
continuos, el valor imputado es el promedio ponderado de las observaciones que no faltan, 
en donde los pesos son las proximidades. Y, para predictores categóricos, el valor imputado 
es la categoría con la mayor proximidad promedio.

El modelo posee los siguientes hiperparámetros:
• mtry = 4
• trees = 2000
• min_n = 40

A través de los gráficos podemos observar como independiente al valor del min_n y ante un 
alto número de trees, la importancia del número de predictores seleccionados (mtry) en el 
valor del acurracy es casi nula. Sin embargo, cuando solo se utiliza solo 1 tree, el mtry 
comienza a tener implicancia en el acurracy (considerando el hecho de que al utilizar solo 1 
tree, de por sí el acurracy disminuye).
Como se mencionó, el método del bagging tiene el problema de la alta correlación entre sus 
árboles. Random forest logra decorrelacionar los árboles por medio una selección aleatoria 
de m predictores (mtry) antes evaluar cada nodo de decisión. Por consiguiente, ciertos 
nodos de decisión utilizarán predictores diferentes al predictor influyente y como 
consecuencia se debería lograr una disminución de la varianza, por ende, un mayor 
acurracy.
Como se puede ver, al ajustar según la métrica de roc_auc se obtiene el mismo accuracy y 
resultados que en el caso de bagging con una accuracy de 94,2% y un 5,8% de error de 
clasificación. Además, tampoco predice strokes.Esto nos indica que es posible que no sea la 
mejor métrica para este caso.
