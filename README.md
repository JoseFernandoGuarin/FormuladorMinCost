# Formulador Lechero FCA

Facultad de Ciencias Agrarias · Universidad de Antioquia
José Fernando Guarin M., Ph.D. · Profesor Asociado

https://josefernandoguarin.github.io/FormuladorMinCost/

https://josefernandoguarin.github.io/FormuladorMinCost/Formulador-Lechero-FCA-Manual.pdf

Aplicación web para formular raciones de ganado de leche a **mínimo costo** con programación lineal. Es un solo archivo HTML: no necesita instalación, servidor ni conexión a internet.

## Cómo abrirla

- **Local:** abra `formulador-lechero.html` con doble clic en cualquier navegador moderno (Chrome, Edge, Firefox). Sin internet funciona igual; solo cambian las fuentes tipográficas.
- **En línea:** use el enlace publicado (versión privada; para compartirla, use el menú Compartir de la página).

## Flujo de trabajo

1. **Animal y consumo.** Escriba los datos de la vaca o del lote: peso vivo, leche, grasa, proteína, lactosa, días en leche, condición corporal, paridad y precio de la leche. Elija la ecuación de consumo de materia seca (CMS) o escriba un valor manual.
2. **Ingredientes.** Marque los alimentos disponibles, ajuste precios ($/kg de materia natural) y ponga límites Mín/Máx en % de la MS, kg de MS o kg de MN. Deje en blanco lo que no quiera restringir; para fijar una cantidad, ponga el mismo valor en Mín y Máx.
3. **Nutrientes.** Ponga mínimos y máximos de concentración en la MS (PB, FDN, ENL, almidón, minerales…), límites por día o por % del peso vivo, y relaciones como Ca:P.
4. **Formular a mínimo costo.** Pulse el botón verde. El resultado aparece en la pestaña **Resultados**.

También puede escribir a mano los kg MN de cada ingrediente y pulsar **Evaluar la dieta escrita a mano** para revisar una ración sin optimizar.

## Qué muestra el resultado

- **Dieta:** kg MN, kg MS, % de la MS, costo por ingrediente y por vaca/día.
- **Composición:** cada nutriente frente a sus límites, con estado *Cumple*, *En el límite* o *No cumple*.
- **Precio sombra:** cuánto cambia el costo diario si un límite sube una unidad. Indica qué restricciones encarecen la dieta.
- **Precio de entrada:** precio al que un alimento que no se usó empezaría a entrar en la dieta.
- **Indicadores:** costo por kg de MS y de leche, ingreso sobre costo alimentario, balance de ENL, forraje en la MS, FDN de forraje / PV, DCAD, tasa de pasaje y CMS predicho por NASEM 2021 (ecuación 2).

Si no existe una dieta que cumpla todo, el programa muestra la que menos se sale de los límites y marca en rojo las restricciones que chocan entre sí.

## Modelo

- Variables: kg de MS de cada ingrediente por vaca/día.
- Objetivo: minimizar el costo diario = SUMAPRODUCTO(kg MS; precio / MS).
- Total de la dieta = CMS (con tolerancia opcional).
- Nutrientes: SUMAPRODUCTO(kg MS; nutriente) / CMS entre Mín y Máx, escrito en forma lineal.
- Relaciones linealizadas: Ca/P ≥ 1,2 se resuelve como Ca − 1,2·P ≥ 0.
- Almidón degradable en rumen: almidón × kd / (kd + kp). Como kp depende del consumo de forraje y concentrado, el programa resuelve, recalcula kp y repite hasta que no cambia.
- Solver: símplex de dos fases incluido en la página.

La pestaña **Método y equivalencias** detalla las fórmulas, las ecuaciones de consumo y cómo quedó cada restricción del Solver de la planilla original.

## Objetivos: costo y potencial de producción

En **Animal y consumo › Objetivo de la formulación** se elige qué optimiza el botón **Formular**:

- **Mínimo costo:** la dieta más barata que cumple las restricciones.
- **Mínimo costo para una producción meta:** además, el potencial de leche de la dieta debe llegar a la meta (kg/d).
- **Máximo ingreso sobre costo alimentario (IOFC):** maximiza precio de la leche × potencial − costo, sin pasar el techo de producción del lote.

El **potencial de producción** es la leche que permite la dieta: el menor valor entre la leche permitida por energía (ENL, NASEM 2021) y la permitida por proteína metabolizable (NRC 2001 simplificado). La app indica cuál de los dos es el nutriente limitante.

La pestaña **Costo vs. producción** calcula la **frontera de Pareto**: dietas de mínimo costo para potenciales de leche crecientes, desde la dieta más barata hasta el máximo posible. Para cada punto muestra el costo, el costo de cada kg extra de leche y el ingreso sobre costo, y marca el punto de mayor IOFC. **Usar esta dieta** lleva ese punto a la formulación.

Supuestos: el CMS queda fijo, la proteína metabolizable usa coeficientes simplificados y el ingreso supone que la vaca produce el potencial de la dieta. Sirve para comparar dietas, no para predecir la producción exacta.

## Informe de la dieta

La pestaña **Informe** arma un informe de tres hojas tamaño carta/A4 con el logosímbolo y los colores de la Universidad de Antioquia. Lleva como responsable técnico a José Fernando Guarin M., Ph.D., Profesor Asociado de la Facultad de Ciencias Agrarias; ese dato se puede editar.

1. **Descripción de los animales y composición de la dieta.** Indicadores clave (costo, costo por kg de leche, ingreso sobre costo, CMS, leche potencial y nutriente limitante), tabla de ingredientes en materia natural y seca con costos, y balance de energía (ENL) y proteína metabolizable con barras de aporte frente a exigencia.
2. **Desempeño.** Productivo, económico, consumo y estimaciones de proteína. Incluye las **restricciones que determinan el costo**, con su precio sombra, y las observaciones del asesor.
3. **Niveles nutricionales.** Agrupados en energía, proteínas, fibras, macrominerales (con DCAD), microminerales y vitaminas, con mínimos, máximos, referencia y estado de cada uno.

Los datos de la hacienda, el productor, el lote, la raza y las observaciones se escriben en la misma pestaña. **Imprimir o guardar como PDF** funciona al abrir `formulador-lechero.html` en el navegador. En la versión en línea ese botón no aparece, porque el visor no permite imprimir; allí se puede copiar el informe para Excel.

## Biblioteca de alimentos

- 107 alimentos con composición en base seca (MS en base natural). Los porcentajes se escriben como porcentaje (17 = 17 %).
- Puede editar valores, crear alimentos nuevos y marcar cuáles son forraje (afecta la tasa de pasaje y los grupos de forraje).
- **Importar / exportar:** la biblioteca en CSV (separador «;», para Excel) y el escenario completo en JSON. En la versión en línea, copie el texto del cuadro; en la versión local también se descarga el archivo.

## Datos guardados

Los cambios se guardan automáticamente en el navegador del usuario (no se envían a ningún servidor). Si borra los datos del navegador o usa otro equipo, se pierden: exporte el escenario en JSON para conservarlo. **Restaurar ejemplo** vuelve a la biblioteca y a la dieta de confinamiento de ejemplo.

## Notas

- Los precios del ejemplo son ilustrativos; reemplácelos por precios locales.
- Los requerimientos de ENL cubren mantenimiento y producción de leche (NASEM 2021); no incluyen gestación ni cambio de peso.
- Los límites del ejemplo son un punto de partida para la clase, no una recomendación nutricional para un hato específico.

