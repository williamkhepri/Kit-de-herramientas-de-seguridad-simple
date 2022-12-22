# **[*Nombre del proyecto*]** Plan de respuesta a incidentes

## *CONFIDENCIAL - NO COMPARTIR*
No compartas esto con nadie que no necesite saberlo

## Comunicación

Canal(es) de War Room: **[*Canal de Discord, grupo de Signal, Zoom, etc.*]**

Participantes de War Room: **[*Nombres de información de contacto*]**

## Pasos inmediatos
- [ ] Revisar las transacciones de explotación para identificar la vulnerabilidad - **[*Persona responsable*]**
  - Herramientas utilizadas
    - [Rastreo/depurador de reproducción de transacciones de función](https://book.getfoundry.sh/reference/cast/cast-run.html#cast-run)
    - [Depurador tierno](https://dashboard.tenderly.co/tx/mainnet/0xf427afc17bd30a84f4b47dc2eaa176115cf28bdea1110245d3b0948ca3b6595c/debugger)
    - [ethtx.info](https://ethtx.info)
- [ ]  Pausar contratos (si es posible), tomar otra acción defensiva, considerar acción ofensiva (es decir, rescates de whitehat) - **[*Persona responsable de la coordinación*]**
  - Pasos:
    - **[*Quién, cómo, qué direcciones*]**
  - Revisar transaccion(es) - **[*Persona responsable, **debe ser diferente de quien creó la transacción***]**
  - NO quieres estar luchando para averiguar quién puede firmar para tomar medidas defensivas, considera acciones ofensivas (es decir, rescatres de whitehat) - **[*Persona responsable de la coordinación*]**
  - Pasos:
        - **[*Quién, cómo, qué direcciones*]**
    - Revisa transacción(es) - **[*Persona responsable, **debe ser diferente de quien creó la transacción***]**
    - NO quieres estar luchando para averiguar quién puede firmar para tomar medidas defensivas. Se recomienda encarecidamente el uso de [OpenZeppelin Defender](https://www.openzeppelin.com/defender), así como la preparación previa de secuencias de comandos de acción defensiva que se pueden implementar según la [Lista de verificación de seguridad previa al lanzamiento](https://github.com/williamkhepri/lista-de-verificación-de-seguridad-previa-al-lanzamiento.md).
- [ ] Revisar todos los contratos para identificar vulnerabilidades en cadena. Deténgalos según sea necesario - **[*Persona responsable*]**
- [ ] Actualizar la interfaz de usuario para reflejar el estado actual - **[*Persona responsable*]**
- [ ] Contactar con socios de seguridad - **[*Persona responsable*]**
    - **[*Lista de auditores anteriores y su información de contacto o ubicación del canal compartido*]**
        - Sus auditores querrán ayudar en la medida de sus posibilidades, aunque sea principalmente para proteger su propia reputación.
    - NO DEJES QUE NADIE FUERA DE TU CÍRCULO DE CONFIANZA ENTRE EN LA SALA DE GUERRA
- [ ] Publicar mensajes a usuarios en Discord - **[*Persona responsable*]**
    - Actualice regularmente a medida que haya nueva información o desarrollos significativos disponibles
        - Incluso si no se sabe nada nuevo, las actualizaciones cada 4 a 12 horas ayudarán a tranquilizar a su comunidad de que está trabajando para abordar la situación.
        - Ejecute todos los mensajes de los revisores de vulnerabilidades para asegurarse de que no se comparta información que inadvertidamente ponga en riesgo la seguridad o se comprometa a una reparación específica antes de que se conozcan todos los hechos.
- [ ] Publicar mensaje a usuarios en Twitter - **[*Responsable*]**
    - Actualice regularmente a medida que haya nueva información o desarrollos significativos disponibles
        - Incluso si no se sabe nada nuevo, las actualizaciones cada 4 a 12 horas ayudarán a tranquilizar a su comunidad de que está trabajando para abordar la situación.
        - Ejecute todos los mensajes de los revisores de vulnerabilidades para asegurarse de que no se comparta información que inadvertidamente ponga en riesgo la seguridad o se comprometa a una reparación específica antes de que se conozcan todos los hechos.

## Después de abordar los pasos inmediatos

- [ ] Redactar y publicar una autopsia pública completa - **[*Persona responsable*]**
- [ ] Preparar el parche para los contratos, idealmente siguiendo las pautas del [proceso de desarrollo](development-process.md) - **[*Persona responsable*]**
    - [ ] Hacer que el parche sea revisado y aprobado por auditores anteriores
    - [ ] Haga que el parche sea revisado por tantos miembros confiables del equipo y la comunidad como sea posible
    - [ ] Si el parche o las interacciones potenciales son lo suficientemente complejas como para justificarlo, considera seriamente un concurso corto de [Code4rena](https://code4rena.com/) (puede iniciarse dentro de las 48 horas)
- [ ] Implementar parche - **[*Persona responsable*]**
