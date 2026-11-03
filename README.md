# FormuladorMinCost
Formulador a mínimo costo con ingredientes tropicales basado en NASEM 2021

https://josefernandoguarin.github.io/FormuladorMinCost/

Formulador Lechero FCA
Facultad de Ciencias Agrarias · Universidad de Antioquia

Aplicación web para formular raciones de ganado de leche a mínimo costo con programación lineal. Es un solo archivo HTML: no necesita instalación, servidor ni conexión a internet.

Cómo abrirla
Local: abra formulador-lechero.html con doble clic en cualquier navegador moderno (Chrome, Edge, Firefox). Sin internet funciona igual; solo cambian las fuentes tipográficas.
En línea: use el enlace publicado (versión privada; para compartirla, use el menú Compartir de la página).
Flujo de trabajo
Animal y consumo. Escriba los datos de la vaca o del lote: peso vivo, leche, grasa, proteína, lactosa, días en leche, condición corporal, paridad y precio de la leche. Elija la ecuación de consumo de materia seca (CMS) o escriba un valor manual.
Ingredientes. Marque los alimentos disponibles, ajuste precios ($/kg de materia natural) y ponga límites Mín/Máx en % de la MS, kg de MS o kg de MN. Deje en blanco lo que no quiera restringir; para fijar una cantidad, ponga el mismo valor en Mín y Máx.
Nutrientes. Ponga mínimos y máximos de concentración en la MS (PB, FDN, ENL, almidón, minerales…), límites por día o por % del peso vivo, y relaciones como Ca:P.
Formular a mínimo costo. Pulse el botón verde. El resultado aparece en la pestaña Resultados.
También puede escribir a mano los kg MN de cada ingrediente y pulsar Evaluar la dieta escrita a mano para revisar una ración sin optimizar.

Qué muestra el resultado
Dieta: kg MN, kg MS, % de la MS, costo por ingrediente y por vaca/día.
Composición: cada nutriente frente a sus límites, con estado Cumple, En el límite o No cumple.
Precio sombra: cuánto cambia el costo diario si un límite sube una unidad. Indica qué restricciones encarecen la dieta.
Precio de entrada: precio al que un alimento que no se usó empezaría a entrar en la dieta.
Indicadores: costo por kg de MS y de leche, ingreso sobre costo alimentario, balance de ENL, forraje en la MS, FDN de forraje / PV, DCAD, tasa de pasaje y CMS predicho por NASEM 2021 (ecuación 2).
Si no existe una dieta que cumpla todo, el programa muestra la que menos se sale de los límites y marca en rojo las restricciones que chocan entre sí.

Modelo
Variables: kg de MS de cada ingrediente por vaca/día.
Objetivo: minimizar el costo diario = SUMAPRODUCTO(kg MS; precio / MS).
Total de la dieta = CMS (con tolerancia opcional).
Nutrientes: SUMAPRODUCTO(kg MS; nutriente) / CMS entre Mín y Máx, escrito en forma lineal.
Relaciones linealizadas: Ca/P ≥ 1,2 se resuelve como Ca − 1,2·P ≥ 0.
Almidón degradable en rumen: almidón × kd / (kd + kp). Como kp depende del consumo de forraje y concentrado, el programa resuelve, recalcula kp y repite hasta que no cambia.
Solver: símplex de dos fases incluido en la página.
La pestaña Método y equivalencias detalla las fórmulas, las ecuaciones de consumo y cómo quedó cada restricción del Solver de la planilla original.

Biblioteca de alimentos
107 alimentos con composición en base seca (MS en base natural). Los porcentajes se escriben como porcentaje (17 = 17 %).
Puede editar valores, crear alimentos nuevos y marcar cuáles son forraje (afecta la tasa de pasaje y los grupos de forraje).
Importar / exportar: la biblioteca en CSV (separador «;», para Excel) y el escenario completo en JSON. En la versión en línea, copie el texto del cuadro; en la versión local también se descarga el archivo.
Datos guardados
Los cambios se guardan automáticamente en el navegador del usuario (no se envían a ningún servidor). Si borra los datos del navegador o usa otro equipo, se pierden: exporte el escenario en JSON para conservarlo. Restaurar ejemplo vuelve a la biblioteca y a la dieta de confinamiento de ejemplo.

Notas
Los precios del ejemplo son ilustrativos; reemplácelos por precios locales.
Los requerimientos de ENL cubren mantenimiento y producción de leche (NASEM 2021); no incluyen gestación ni cambio de peso.
Los límites del ejemplo son un punto de partida para la clase, no una recomendación nutricional para un hato específico.
