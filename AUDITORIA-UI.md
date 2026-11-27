# Auditoría de UI de Cuéntame, pantalla por pantalla

Recorrido completo de la app con sesión iniciada y datos sembrados, tomado contra el build web servido en local con emuladores de Auth y Firestore (390x844, como un teléfono). Las capturas están en `assets/auditoria-ui/capturas/`, referenciadas con ruta relativa.

Los hallazgos marcados "Expo Go" hay que confirmarlos en el dispositivo nativo: la web es solo una de las tres metas (nativo, Android, web).

## Hallazgos (resumen)

1. **Montos ocultos inconsistente.** Al activar "Ocultar montos", algunas cifras siguen visibles en pantalla. Evidencia: `pantalla051-inicio-montos-ocultos.png`, `pantalla061-inicio-montos-ocultos.png`, `pantalla071-inicio-montos-ocultos.png`. Expo Go.
2. **Formato de negativos distinto entre pantallas.** El mismo saldo negativo se muestra con distinto estilo según la pantalla. Evidencia: carteras vs. deudas. Expo Go.
3. **Íconos de carteras en color distinto según la pantalla.** Evidencia: `pantalla012-carteras.png` frente a `pantalla032-cuenta-miscarteras.png`.
4. **Agente sin respuesta útil.** Las preguntas del agente devuelven respuesta genérica o nada. Evidencia: `pantalla025-agente-respuesta.png`, `pantalla078-agente-respuesta.png`.
5. **Porcentaje pegado al monto en el desglose por categoría.** Se lee "$ 211.7933%", "$ 164.4926%", etc.: falta separación entre monto y porcentaje. Evidencia: `pantalla0111-inicio-desglose.png`.
6. **Color del monto en el editor de transacción.** Un gasto se muestra con monto verde en el editor (`pantalla0114-transaccion-detalle-dia22.png`) y en rojo en la lista (`pantalla096-transacciones-dia23-lista.png`). Criterio a unificar: gasto rojo, ingreso verde.
7. **En web, tocar las pestañas desde Inicio recarga y vuelve a Inicio.** "Transacciones", "Ver informes" y "Ver detalles" no se aprecian como navegación en el build web. Confirmar en Expo Go si es nativo o solo de la exportación web.
8. **El calendario solo muestra la semana corriente.** El día 22 (con movimientos) no se alcanza tocando; habría que avanzar semanas. Evidencia: `pantalla075-transacciones-calendario.png`, `pantalla0108-calendario-dia23.png`.

## 1. Entrada y onboarding

Splash y seis pasos de onboarding. Textos legibles, ritmo correcto, nada cortado.

![splash](assets/auditoria-ui/capturas/pantalla001-entrada-splash.png)
![onboarding 1](assets/auditoria-ui/capturas/pantalla001-onboarding.png)
![onboarding 2](assets/auditoria-ui/capturas/pantalla002-onboarding.png)
![onboarding 3](assets/auditoria-ui/capturas/pantalla003-onboarding.png)
![onboarding 4](assets/auditoria-ui/capturas/pantalla004-onboarding.png)
![onboarding 5](assets/auditoria-ui/capturas/pantalla005-onboarding.png)
![onboarding 6](assets/auditoria-ui/capturas/pantalla006-onboarding.png)

Controles: botón SALTAR en todos los pasos, y "Continuar". El salto funciona y lleva directo a login.

## 2. Login y bienvenida

Botón "Continuar con Google" claro y centrado. Tras el popup de Auth, la app aterriza en la pantalla de bienvenida con saldo y accesos.

![login](assets/auditoria-ui/capturas/pantalla007-login.png)
![bienvenida](assets/auditoria-ui/capturas/pantalla008-bienvenida.png)
![bienvenida con inicio](assets/auditoria-ui/capturas/pantalla008-bienvenida-inicio.png)
![post login](assets/auditoria-ui/capturas/pantalla008-post-login.png)

Observación: entre el cierre del popup y el primer contenido hay una espera sin indicador de carga (web). Verificar en Expo Go si aparece el spinner nativo.

## 3. Inicio

Resumen del mes, totales, desglose por categoría, transacciones recientes y accesos rápidos. Con datos y sin datos se ve coherente.

![inicio con datos](assets/auditoria-ui/capturas/pantalla009-inicio-con-datos.png)
![inicio](assets/auditoria-ui/capturas/pantalla010-inicio.png)
![inicio base](assets/auditoria-ui/capturas/pantalla051-inicio-base.png)

### 3.1 Montos ocultos

El toggle "Ocultar montos" funciona, pero deja cifras visibles en algunos bloques (hallazgo 1).

![montos ocultos](assets/auditoria-ui/capturas/pantalla051-inicio-montos-ocultos.png)
![montos restaurados](assets/auditoria-ui/capturas/pantalla052-inicio-montos-restaurados.png)

### 3.2 Toggle Mes / Semana

Ambos estados se pintan bien y los totales cambian de ventana.

![mes](assets/auditoria-ui/capturas/pantalla053-inicio-toggle-mes.png)
![semana](assets/auditoria-ui/capturas/pantalla054-inicio-toggle-semana.png)

### 3.3 Desglose y cierre de página

El desglose por categoría con barras es correcto salvo el porcentaje pegado al monto (hallazgo 5). La lista de transacciones recientes usa rojo para gastos y verde para ingresos.

![desglose](assets/auditoria-ui/capturas/pantalla0111-inicio-desglose.png)
![fin de inicio](assets/auditoria-ui/capturas/pantalla0112-inicio-final.png)

## 4. Transacciones

Lista principal con filtros, calendario semanal, ayuda y selector de cartera.

![transacciones](assets/auditoria-ui/capturas/pantalla011-transacciones.png)
![transacciones base](assets/auditoria-ui/capturas/pantalla072-transacciones-base.png)

### 4.1 Ayuda y avanzar día

La ayuda "?" explica la pantalla. El botón »» avanza al día siguiente y actualiza la lista.

![ayuda](assets/auditoria-ui/capturas/pantalla073-transacciones-ayuda.png)
![dia avanzado](assets/auditoria-ui/capturas/pantalla074-transacciones-dia-avanzado.png)

### 4.2 Calendario y día seleccionado

El calendario marca con puntos los días con movimientos. Ver hallazgo 8.

![calendario](assets/auditoria-ui/capturas/pantalla075-transacciones-calendario.png)
![calendario dia 23](assets/auditoria-ui/capturas/pantalla0108-calendario-dia23.png)
![lista del dia 23](assets/auditoria-ui/capturas/pantalla096-transacciones-dia23-lista.png)

### 4.3 Selector de cartera

El selector filtra por cartera y la lista responde bien.

![selector cartera](assets/auditoria-ui/capturas/pantalla077-transacciones-selector-cartera.png)

## 5. Editor y formulario de transacción

El modal de edición carga categoría, cartera, nota y fecha de la transacción. En el formulario nuevo, los campos obligatorios quedan claros.

![editar transaccion](assets/auditoria-ui/capturas/pantalla041-editar-transaccion.png)
![detalle dia 22](assets/auditoria-ui/capturas/pantalla0114-transaccion-detalle-dia22.png)
![detalle](assets/auditoria-ui/capturas/pantalla097-transaccion-detalle.png)
![nueva transaccion](assets/auditoria-ui/capturas/pantalla063-nueva-transaccion-form.png)

Observación: color del monto inconsistente con la lista (hallazgo 6). El botón "Guardar" es grande y alcanzable con el pulgar.

## 6. Carteras y categorías

Lista de carteras con saldos y su formulario de creación. Igual para categorías.

![carteras](assets/auditoria-ui/capturas/pantalla012-carteras.png)
![nueva cartera](assets/auditoria-ui/capturas/pantalla013-nueva-cartera.png)
![cartera nueva](assets/auditoria-ui/capturas/pantalla043-cartera-nueva.png)
![categorias](assets/auditoria-ui/capturas/pantalla014-categorias.png)
![nueva categoria](assets/auditoria-ui/capturas/pantalla015-nueva-categoria.png)

Observaciones: hallazgos 2 y 3 (negativos y color de íconos).

## 7. Informes

Los accesos "Ver informes" y "Ver detalles" abren la vista de informes con su detalle.

![informes](assets/auditoria-ui/capturas/pantalla044-informes.png)
![informes acceso](assets/auditoria-ui/capturas/pantalla0101-informes.png)
![informe detalle](assets/auditoria-ui/capturas/pantalla045-informe-detalle.png)
![informe detalle 2](assets/auditoria-ui/capturas/pantalla0102-informe-detalle.png)

Observación: en web la navegación desde Inicio se aprecia mal (hallazgo 7).

## 8. Agente

El chip del agente abre la conversación; las respuestas no aportan datos reales (hallazgo 4).

![agente](assets/auditoria-ui/capturas/pantalla016-agente.png)
![agente chip](assets/auditoria-ui/capturas/pantalla024-agente.png)
![agente pregunta](assets/auditoria-ui/capturas/pantalla056-agente-pregunta.png)
![agente respuesta](assets/auditoria-ui/capturas/pantalla025-agente-respuesta.png)
![agente respuesta 2](assets/auditoria-ui/capturas/pantalla078-agente-respuesta.png)

## 9. Deudas y préstamos

Lista con totales y estado de cada deuda.

![deudas](assets/auditoria-ui/capturas/pantalla017-deudas.png)
![deudas desde cuenta](assets/auditoria-ui/capturas/pantalla058-cuenta-deudas.png)

## 10. Cuenta

Los cinco acordeones: Mis carteras, Deudas y préstamos, Categorías, Soporte, Configuraciones y Acerca de. Todos abren y cierran bien; el contenido coincide con lo esperado.

![cuenta](assets/auditoria-ui/capturas/pantalla018-cuenta.png)
![miscarteras](assets/auditoria-ui/capturas/pantalla057-cuenta-miscarteras.png)
![deudas y prestamos](assets/auditoria-ui/capturas/pantalla080-cuenta-deudasyprestamos.png)
![categorias cuenta](assets/auditoria-ui/capturas/pantalla059-cuenta-categorias.png)
![soporte](assets/auditoria-ui/capturas/pantalla060-cuenta-soporte.png)
![configuraciones](assets/auditoria-ui/capturas/pantalla061-cuenta-configuraciones.png)
![acerca](assets/auditoria-ui/capturas/pantalla062-cuenta-acerca.png)

## 11. Navegación inferior

Los cuatro destinos del tab bar (Inicio, Transacciones, Agente, Cuenta) más el botón central "+". El "+" abre el formulario de transacción.

![nav inicio](assets/auditoria-ui/capturas/pantalla-nav-inicio.png)
![nav transacciones](assets/auditoria-ui/capturas/pantalla-nav-transacciones.png)
![nav agente](assets/auditoria-ui/capturas/pantalla-nav-agente.png)
![nav cuenta](assets/auditoria-ui/capturas/pantalla-nav-cuenta.png)

## Pendiente de verificar en Expo Go

- Hallazgos 1, 2, 3 y 7 (pueden ser particulares del build web).
- Hápticos, transiciones y navbar en dedo, que la web no muestra.
