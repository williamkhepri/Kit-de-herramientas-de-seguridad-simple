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
 13. Revisión de relaciónes públicas
   - 
