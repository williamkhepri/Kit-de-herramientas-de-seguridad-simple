# Lista de verificación de preparación para auditorías

### Lista de verificación de calidad mínima básica

- [ ] Utiliza la [última](https://docs.soliditylang.org/en/latest/) versión principal de Solidity.
- [ ] Utiliza bibliotecas conocidas / establecidas cuando sea posible. Se prefieren los [contratos OpenZeppenlin](https://github.com/OpenZeppelin/openzeppelin-contracts/) porque priorizan la seguridad por encima de todo, y la mayoría de los auditores ya están familiarizados con ellos. [Los contratos de Solmate](https://github.com/Rari-Capital/solmate) pueden ser una buena alternativa para funciones en las que la optimización del gas es primordial.
- [ ] Los contratos se compilan sin errores ni advertencias del compilador.
- [ ] Documentar todas las funciones. Usa [Documentación de NetSpec] (https://docs.soliditylang.org/en/develop/natspec-format.html) para funciones `publicas`/`externas`. Considera esta parte de la interfaz pública del contrato.
- [ ] Cualquier función `pública` que pueda hacerse `externa` debe hacerse `externa`. Esto no es solo una consideración de gas, sino que también reduce la sobrecarga cognitiva para los auditores porque reduce la cantidad de contextos posibles en los que se puewde llamar a la función.
- [ ] Ten pruebas para todas las historias de usuario del "camino feliz". Todas las pruebas deben pasar.
- [ ] Usa el [patrón de interacciones de efectos de verificación](https://docs.soliditylang.org/en/v0.8.13/security-considerations.html#use-the-checks-effects-interactions-pattern) siempre que sea posible. De lo contrario, utilice protecciones de reentrada. Trata todas las transferencias de tokens y ETH como "interacciones".
- [ ] Evita el uso de ensamblador tanto como sea posible. El uso de ensamblaje aumenta los tiempos de auditoría porque desecha las barreras de seguridad de Solidity y debe verificarse con mucho más cuidado.
- [ ] Documentar el uso de `unchecked`. Describe concretamente *por qué* es seguro no realizar comprobaciones aritméticas en cada bloque de código. Preferiblemente para cada operación.
- [ ] Ejecute el código a través de un corrector ortográfico.
- [ ] Ejectute una herramienta de análisis estático preferiblemente ([Slither](https://github.com/crytic/slither)) o [MythX](https://mythx.io/) como alternativa en tu código y considera los resultados que muestre. Muy a menudo levantan banderas para no tener problemas, pero a veces pueden sacar cosas que pueden resultar interesantes, por lo que vale la pena realizarlos.
- [ ] Haz que al menos un desarrollador confiable de Solidity o una persona de seguridad de fuera de la organización verifique "la cordura" de tus contratos. Si tu código es "un incendio" y necesita cambios importantes, querrás saberlo pronto y no de un amigo de confianza (gratis) en lugar de después de una costosa auditoría.

### Es bueno tener

- [ ] Si usas [Foundry](https://github.com/foundry-rs/foundry), te recomiendo **encarecidamente** hacer pruebas de fuzzing. Hay una sobrecarga adicional baja y una mayor probabilidad de encontrar errores usando fuzzing. Si estas utilizando hardhat, considera agregar Foundry para esta funcionalidad.
- [ ] Escribir pruebas negativas. Por ejemplo, si los usuarios NO deberías poder retirar dinero dentro de los 100 bloques posteriores al depósito, escribe una prueba en que un usuario intente retirarlos antes y asegúrate de que dicho intento falle.
- [ ] También puedes intentar usar herramientas de verificación formal para verificar los invariantes, pero ten en cuenta que, en la práctica, las herramientas de verificación formal actuales rar vez son útiles para algo que no sea trivial.
- [ ] Escribe tus supuestos de seguridad. Esto no tiene que ser demasiado formal. Por ejemplo, "Suponemos que el `propietario` no es malicioso, que los oráculos de Chainlink no mentirán sobre el precio del token, que los oráculos de Chainlink siempre informarán el precio al menos una vez cada 24 horas, que todos los tokens que el `propietario` aprueba son tokens compatibles con ERC20 sin enlaces de transferencia, y que nunca habrá una reorganización en cadena de más de 30 bloques". Esto lo ayuda a comprender cómo las cosas podrían salir mal *incluso si sus contratos están libres de errores*. Los auditores también pueden decirle si sus suposiciones son realistas o no. También pueden señalar suposiciones que está haciendo y que no se dio cuenta de que estaba haciendo.
- [ ] Si no estás seguro acerca de algo en tu propio código, o si hay áreas en las que te gustaría que los auditores pasaran más tiempo, haz una lista para compartirlas con los auditores.
