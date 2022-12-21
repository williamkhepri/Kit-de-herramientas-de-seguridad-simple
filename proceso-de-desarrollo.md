# Proceso de desarrollo
Uno de los factores más cruciales para tener un código base seguro es un proceso de desarrollo sólido: "mas vale prevenir que curar." Este documento proporciona un proceso de desarrollo de *ejemplo* que hemos encontrado que funciona bien.
Además de mejorar la calidad general de código, este proceso sirve como complemento de nuestra [lista de verificación de preparación para auditorías](#). Si sigues el proceso acontinuación, naturalmente deberías marcar la mayoría de los elementos de verificación.

```
Feature Request
 |
 └> Specification
     |
     └> Evaluation (time + complexity + risks)
        |
        └> Implementation
           |
           └> Testing
              |
              └> Deployment
                 |
                 └> Monitoring
```
## Especificación

Toma nota de los tipos de variables que afectan a las características:
 1. ¿Entrada de usuario?
 2. ¿Hora?
 3. ¿Otros protocolos?
 4. ¿Estado existente?

## Evaluación

Dedica tiempo para evaluar:
 1. ¿Cuánto tiempo tomará esto de forma realista? Sin especificación, la mayoría de las estimaciones serán incorrectas.
 2. ¿Es la especificación demasiado compleja? La complejidad conduce a errores y peor código en general
 3. ¿Cuáles son los riesgos asociados con esta función? Dedica suficiente tiempo a evaluar esto. Considera cada módulo que la característica pueda "tocar". Luego, vuelve a revisar los módulos excluidos y *asegúrate* de que no se vean afectados.

Es posible que ya tengas mucho dinero en riesgo, o lo tengas después del lanzamiento. Como tal, **debes** considerar cómo cada característica puede poner *todo* en riesgo. El coste de un paso en falso probablemente será demasiado alto.

## Implementación
1. Redactar un PR
  - Incluye la solicitud de función + especificación.
2. Escribir una implementación inicial
  - Obtener una implementación inicial en su lugar.
  - Documentar el comportamiento previsto de todas las funciones (NatSpec para funciones públicas/externas) y agregar documentación/comentarios en líena
  - Cualquier uso de ``unchecked`` debe incluir documentación tipo ``safety`` de la siguiente forma:
``
// Safety:
//  1. a + b: a and b are uint64s, and a is casted up to uint128, so a uint128(uint64) + uint64
//     cannot overflow
//  2. c * 2: c is casted up to uint256, so a uint256(uint128) * 2 cannot overflow
uint256 d;
unchecked {
  uint128 c = uint128(a) + b;
  d = uint256(c) * 2;
}
``
- **Cada** linea de montaje está documentada/comentada

## Pruebas

 3. Escribir [pruebas concretas iniciales](#)
  - Escribe una prueba inicial para la implementación. Cada escritura en el almacenamiento debe verificarse, cada reversión debe verificarse. Línea por línea de la implementación, busca escrituras de almacenamiento.
  - Para funciones que no cambian de estado, una buena práctica es tener un contrato de prueba que herede el contrato que hace los cálculos
  - Utilizar herramientas de cobertura (como la herramienta de cobertura de [Foundry](https://github.com/foundry-rs/foundry)) para ver cómo de bien cubren tus pruebas a tu código.

 4. Limpiar la implementación
 5. Mejorar las pruebas, luego volver al paso 4 en función de los errores encontrados
 6. Ejecutar [slither](https://github.com/crytic/slither).
 7. Escribir tests de Fuzzing
 8. Volver al paso 4 si se encuentran errores
 9. Escribir pruebas de integración
  - Es probable que ahora tu característica haga exactamente lo que crees que hace. En un sistema complejo, eso no es suficiente. Idealmente, también has podido probar cómo afecta a todo el sistema. Las pruebas invariantes llegarán pronto a Foundry, que que debería ayudar a las pruebas de estilo de integración, pero haz lo que puedas con las prubas de Fuzzing en una base más amplia (no solo para una sola función)
 10. Volver al paso 4 si se encuentran errores.
 11. Documentación de limpieza
 12. Configurar IC
 13. Revisión de PR
   - El implementador es solo la primera línea de defensa. Si eres un revisor, confirma qe el implementador siguió los principios anteriormente descritos (prueba por transición de estado, prueba por reversión, prueba de fuzz y prueba de integración)
   - Revisa la documentación y asgúrate de que la implementación coincida con el comportamiento documentado. Si no es así, comuníquese con el implementador y confirme cuál debe actualizarse.
   - Asegúrate de que CI pase
   - Comprueba pasos en falso comunes ([reentrada, patrones de interacciones de efectos de comprobación, etc.](https://docs.soliditylang.org/en/v0.8.13/security-considerations.html#pitfalls)

## Despliegue
 14. Escriba un script de implementación
   - Escribir el script de implementación
   - Lo más probable es que Foundry tenga listas las secuencias de comandos cuando esté leyendo esto. Echa un vistaszo a este [PR](https://github.com/foundry-rs/foundry/pull/1208)
 15. Escribe una prueba de implementación
   - Asegúrate de que la implementación se realice exactamente según lo planeado escribiendo una prueba que testée *cada transición de estado* y asegúrate de que no ocurran cambios inesperados. Una forma de lograr esto es el código de trucos `record`. Si realizas una actualización a un protocolo existente, crea una lista de las direcciones de todo su protocolo, registra las llamadas y realiza una actualización. Luego, llama a `accesos` para cada dirección de tu protocolo. Asegúrate de que no haya ranuras/direcciones que hayan cambiado inesperadamente.
 16. Ten una auditoría realizada
  - Considera si esta característica/contrato necesita una auditoría. Inclínate siempre hacia estar seguro en vez de lamentar después.
  - Haz que la complejidad y el tamaño del código informen si se debe y quién debe auditar sus contratos.
  - En Nascent puedes encontrar una lista interna de niveles de calidad de los auditores. Debes consultar con otros desarrolladores sobre quiénes son buenos auditores y quiénes no. Algunos auditores solo están allí para marcar una casilla para un protocolo, otros realmente se preocupan por encontrar vulnerabilidades.
 17. Implementa las correcciones de auditoría
 18. Servicio de monitoreo de configuración
  - Ten una herramienta interna que monitorice aspectos importantes de tu sistema.
  - Usa herramientas como [Check the chain](https://github.com/fei-protocol/checkthechain) + [Grafana](https://grafana.com/), o usa una herramienta de monitoreo estándar como [Tenderly](https://tenderly.co/alerting) o [Defender Sentinels](https://www.openzeppelin.com/defender) de Open Zepelin.
 19. Prepara/actualiza tu [Plan de respuesta a incidentes](incident-response-plan-template.md)
 20. Implementar los contratos
  - Felicidades, probablemente acabas de aplastar al 99% de los desarrolladores de Solidity en términos de un desarrollo + implementación segura.

## Monitoring
21. Supervisar las próximas dos horas
 - Usa el servicio de monitoreo que configuraste para observar atentamente comportamientos inesperados y estar listo para tomar medidas.
22. Relájate, tómate una birra, te lo has ganado. :D
