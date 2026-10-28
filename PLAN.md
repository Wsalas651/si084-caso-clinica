**EXAMEN DE LA UNIDAD II**

**CURSO:**
AUDITORIA DE SISTEMAS

**URL GITHUB:**
[https://github.com/Wsalas651/si084-caso-clinica/tree/examen-u2](https://github.com/Wsalas651/si084-caso-clinica/tree/examen-u2)

**ALUMNO:**
2022073896 – Salas Jimenez, Walter Emmanuel

**DOCENTE:**
DR. OSCAR JUAN JIMENEZ FLORES

TACNA — PERÚ

## II INFORMACIÓN DE LA ENTIDAD AUDITADA Y CONOCIMIENTO PREVIO

La Clínica Santa Lucía S.A.C. es una IPRESS privada de nivel II-1 ubicada en Tacna, supervisada por SUSALUD y por la Autoridad Nacional de Protección de Datos Personales. Cuenta con 162 trabajadores y tres personas en Sistemas. Su canal digital, administrado por WebSur S.A.C. desde 2021, permite reservar 3 850 citas mensuales, consultar resultados para 12 400 pacientes con cuenta y recibir postulaciones laborales, que suman 214 en 2026. Sus ingresos declarados de 2025 ascienden a S/ 18 600 000.

La comisión ejecutó el reconocimiento autorizado el 28/10/2026 sobre la simulación local. Los resultados describen la superficie publicada y servirán para definir las pruebas posteriores. No acreditan por sí solos una explotación, el contenido de los respaldos ni el funcionamiento real de las tecnologías anunciadas.

Cuadro 1 Lo declarado por WebSur S.A.C. frente a lo observado

| E | WSTG | Lo observado con cifras | D | Sí/No | Evidencia |
| --- | --- | --- | --- | --- | --- |
| E1 | WSTG-INFO-04 | Respondieron tres aplicaciones con HTTP 200 en 8080, 8081 y 8088. El entorno de pruebas anuncia una copia de producción restaurada el 12/09/2026. La raíz de api-citas 1.4.0 declara entorno de producción y tres endpoints, que no se consultaron. | D1 | No | E1_aplicaciones.txt |
| E2 | WSTG-INFO-02 | Las cabeceras anuncian nginx 1.20.2 y PHP 7.4.3. PHP 7.4 terminó su soporte el 28/11/2022. nginx 1.20 figura como versión heredada y fue sustituida como rama estable por 1.22 el 24/05/2022. Esta última fecha es de sustitución, no una fecha formal de fin de soporte. | D2 | No | E2_servidor.txt |
| E3 | WSTG-INFO-03 | robots.txt enumera cuatro rutas excluidas de indexación, /admin/, /respaldos/, /resultados/ y /wp-content/plugins/wp-file-manager/. security.txt devuelve HTTP 404. Estas exclusiones permiten descubrir rutas y no restringen el acceso. | — | — | E3_metarchivos.txt |
| E4 | WSTG-INFO-05 | Se encontraron cuatro comentarios, uno sobre el desarrollador y su correo, y tres sobre el usuario admin_pruebas, la integración interna http://10.20.0.15:8090/lis/resultados y el patrón ver.php?orden=NNNNN. La página indica que la contraseña inicial es la fecha de nacimiento en formato ddmmaaaa. | D8 | No | E4_contenido.txt |
| E5 | WSTG-INFO-08 | El generador anuncia WordPress 5.8.1, frente al 6.6 del inventario. Cuatro recursos anuncian MedicalPro 2.1.0, Contact Form 7 5.3.1, jQuery 3.4.1 y el recurso elFinder bajo wp-file-manager con ver=6.8. Esta huella requiere corroboración. CVE-2020-35489 afecta a Contact Form 7 anterior a 5.3.2. | D3 | No* | E5_componentes.txt |
| E6 | WSTG-INFO-06 | Existen cuatro formularios. Citas, resultados y postulaciones solicitan datos personales; citas también pide síntomas y especialidad. Postulaciones recibe un CV. Citas emplea GET y los cuatro action usan HTTP. No se envió ningún formulario. | D7 | No | E6_entradas.txt |
| E7 | WSTG-CONF-04<br>WSTG-CONF-05 | El listado público muestra cuatro archivos, un CSV, dos SQL comprimidos .sql.gz y un .bak de configuración. /admin/ devuelve HTTP 200 desde la máquina del auditor. No se descargaron respaldos ni se probó el acceso administrativo autenticado. | D5 y D6 | No | E7_expuestos.txt |
| E8 | WSTG-CONF-07<br>WSTG-SESS-02 | Faltan las cuatro cabeceras revisadas, HSTS, CSP, X-Frame-Options y X-Content-Type-Options. PHPSESSID no incluye Secure, HttpOnly ni SameSite. El portal responde por HTTP. La consulta HTTPS al puerto 443 quedó sin ejecutar por estar fuera de 8080 a 8090. | D4 | No | E8_cabeceras.txt |

* La observación contradice la versión del inventario y no sustenta la actualización mensual declarada. Su cumplimiento durante todo el periodo necesitará registros de cambios. Para E8 se conserva el reconocimiento pedido, pero se omite WSTG-CONF-12 como referencia técnica porque no figura en el índice oficial de WSTG 4.2. Las otras cabeceras se vinculan con ASVS V14.4.

Las ocho salidas están guardadas en evidencias/ y sus sumas se verificaron con SHA256SUMS.txt. La comprobación del puerto 443 constituye una limitación expresa de E8 y no tiene resultado técnico.

Ninguna de las ocho declaraciones queda plenamente respaldada por el reconocimiento. Las contradicciones observadas impiden depositar confianza inicial en las afirmaciones de WebSur sin evidencia adicional. El patrón de resultados mediante orden=NNNNN se relaciona con el incidente del 08/09/2026 y justifica revisar la autorización por paciente y la separación de caché. Sin embargo, el reconocimiento no demuestra que exista acceso a resultados ajenos ni confirma la explicación de caché del proveedor. Ambas hipótesis se evaluarán en la Unidad III.

## III DENOMINACIÓN DE LA MATERIA DE CONTROL

La materia de control será la protección de los datos personales y de salud tratados por el canal digital de la Clínica Santa Lucía durante el periodo del 01/01/2026 al 30/09/2026. La comisión evaluará la autenticación, autorización, sesiones, transmisión y configuración de los servicios publicados. Esta selección responde al incidente de divulgación de un hemograma ajeno y a las observaciones E4, E6 y E8 sobre identificadores de resultados, formularios por HTTP y ausencia de protecciones de sesión.

## IV ALCANCE

IV.1 Incluye. La comisión abarcará el portal http://127.0.0.1:8080/ y sus módulos /citas/, /resultados/, /postula/ y /admin/. Las poblaciones de negocio serán 3 850 citas mensuales, 12 400 pacientes con cuenta y 214 postulaciones declaradas para todo 2026. El volumen mensual permite estimar 34 650 citas en nueve meses, sujeto a conciliación con los registros. Las postulaciones de enero a septiembre se determinarán al recibir el padrón fechado. La cantidad de usuarios administrativos también se solicitará.

Se incluirán el entorno de pruebas http://127.0.0.1:8081/ y la API http://127.0.0.1:8088/, por haber sido encontrados en E1. Se considerará como exposición potencial la copia anunciada de producción, sin asumir que sus registros ni los de la API sean poblaciones adicionales a los pacientes y citas del portal. Se incluirán las cuatro entradas del listado /respaldos/, la configuración de publicación, los componentes anunciados y la relación de WebSur con estos controles. El volumen y contenido efectivo de la copia, los respaldos y la API se corroborarán por documentación autorizada.

IV.2 Periodo auditado. La revisión comprenderá del 01/01/2026 al 30/09/2026, según la solicitud de Gerencia y por incluir el incidente del 08/09/2026. El reconocimiento del 28/10/2026 será evidencia de planificación posterior al periodo. Para atribuir condiciones a los meses auditados se contrastarán versiones, registros y cambios fechados.

IV.3 Exclusiones. Se excluirán la auditoría financiera, la atención médica, los equipos biomédicos y la red corporativa general, porque no responden a la materia de control. El sistema interno de laboratorio 10.20.0.15 se considerará únicamente como integración referenciada, sin conexión técnica por estar fuera de autorización. Se excluirán servicios externos y el dominio .test de las peticiones actuales. En esta fase no se probarán credenciales, formularios, archivos de respaldos ni /v1/. Las pruebas de la Unidad III requerirán aprobación expresa del plan y de sus rutas y operaciones.

IV.4 Limitaciones inherentes. La auditoría aportará seguridad razonable. La simulación publica páginas y cabeceras y no permite acreditar el comportamiento de un sistema clínico real. Para probar autenticación, autorización, caché y cargas se necesitará una réplica funcional autorizada con datos ficticios. La falta de registros históricos limitará conclusiones sobre enero a septiembre. Las muestras dirigidas no tendrán representatividad estadística. No se concluye sobre HTTPS en 443 porque dicho puerto quedó fuera del alcance del reconocimiento.

## V OBJETIVOS

- Determinar si el canal digital de la Clínica Santa Lucía protege los datos personales y de salud durante el periodo auditado, tomando como criterios OWASP WSTG 4.2, OWASP ASVS 4.0.3, los controles aplicables de NTP-ISO/IEC 27001:2022 y la Ley 29733.

- Evaluar la autenticación y gestión de sesiones de pacientes y administradores conforme a WSTG-ATHN-02, WSTG-SESS-02, ASVS V2.3.1 y V3.4.1 a V3.4.3, y NTP-ISO/IEC 27001:2022 A.8.5. ¿Las credenciales iniciales y las cookies reducen el riesgo de suplantación y uso no autorizado de sesiones?

- Determinar si la consulta de resultados restringe cada registro al paciente autorizado y mantiene la separación de caché, conforme a WSTG-ATHZ-04, ASVS V4.2.1 y V8.1.1, NTP-ISO/IEC 27001:2022 A.8.3 y Ley 29733, artículos 9, 16 y 17. ¿Puede un paciente acceder al resultado de otro al cambiar la orden o reutilizar una respuesta almacenada?

- Verificar la protección de la transmisión, publicación de servicios y recepción de archivos conforme a WSTG-CRYP-03, WSTG-CONF-02, WSTG-CONF-04 y WSTG-BUSL-08, ASVS V9.1.1, V14.2.1 y V12.2.1, y NTP-ISO/IEC 27001:2022 A.8.8, A.8.9 y A.8.24. ¿Los servicios y formularios protegen los datos y evitan exposiciones por configuración, componentes o archivos?

## VI ENFOQUE MATERIALIDAD Y RIESGO DE AUDITORÍA

VI.1 Enfoque. La comisión aplicará un enfoque predominantemente sustantivo, mediante comprobaciones directas y revisión documental. E1, E6, E7 y E8 muestran servicios adicionales, formularios por HTTP, listados públicos y ausencia de protecciones. No se presumirá eficacia operativa de controles que no se han identificado. Cuando exista evidencia de un control implementado, se evaluará primero su diseño y después su operación durante el periodo.

VI.2 Materialidad. Como referencia cuantitativa de planificación se propone el 1 % de los ingresos declarados de 2025, equivalente a S/ 186 000 sobre S/ 18 600 000. Este umbral es una decisión del plan y no una exigencia normativa ni un límite para aceptar filtraciones. En lo cualitativo, una sola divulgación no autorizada de datos de salud se considerará material, sin importar su importe, por afectar la seguridad y confidencialidad previstas en los artículos 9, 16 y 17 de la Ley 29733.

VI.3 Riesgo de auditoría. El riesgo inherente se valorará alto por la naturaleza de los datos y la exposición de síntomas y acceso a resultados observada en E4 y E6. El riesgo de control se valorará alto por el listado de respaldos y administración de E7, las cabeceras ausentes y la cookie sin atributos de E8. El riesgo de detección será alto inicialmente por tratarse de reconocimiento limitado, huellas no corroboradas y una simulación sin trazabilidad histórica. La comisión buscará reducirlo con pruebas directas autorizadas, revisión de registros y ampliación de muestras ante excepciones.

## VII PROCEDIMIENTOS DE AUDITORÍA

La comisión ejecutará seis procedimientos en la Unidad III, después de obtener la aprobación del plan y de una réplica funcional con datos ficticios. P-02 y P-03 cubrirán el primer objetivo específico, P-01 responderá al segundo y al incidente, y P-04 a P-06 cubrirán el tercero. Se emplearán seis categorías WSTG distintas. Las muestras serán dirigidas y sus tamaños no se extrapolarán estadísticamente. Cada excepción se fijará antes de ejecutar las pruebas.

## P-01 Acceso a resultados y separación de caché

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Segundo objetivo específico de la sección V |
| Origen en el reconocimiento | E4 revela el patrón ver.php?orden=NNNNN. E1 anuncia una API con resultados. La solicitud registra el incidente del 08/09/2026. |
| Riesgo que cubre | R1 Divulgación del resultado de un paciente a otro. |
| Criterio | WSTG-ATHZ-04 · OWASP ASVS 4.0.3 V4.2.1 y V8.1.1 · NTP-ISO/IEC 27001:2022 A.8.3 · Ley 29733, artículos 9, 16 y 17. |
| Tipo de prueba | Prueba sustantiva de autorización y revisión de caché e incidente. |
| Población y muestra | Población de referencia de 12 400 pacientes con cuenta. Muestra dirigida de tres cuentas ficticias con dos resultados cada una. Incluya el expediente del incidente y sus registros disponibles. Extienda a la API solo si sus endpoints están expresamente autorizados. |
| Evidencia a obtener | Matriz cuenta y orden, solicitudes y respuestas anonimizadas, configuración de caché y documentación del incidente. |
| Definición de excepción | Cualquier respuesta que entregue datos del resultado de otro paciente a una cuenta sin permiso o a una sesión cerrada, incluida una respuesta servida desde caché. La ausencia de registros del incidente se documentará como limitación independiente. |
| Horas estimadas | 4 horas |

1. Obtenga la autorización de la réplica y asigne a tres cuentas ficticias sus seis resultados conocidos.

2. Consulte un resultado propio con la cuenta A y registre la orden y respuesta. Sustituya únicamente la orden por una perteneciente a B y repita con las tres cuentas, sin enumerar identificadores reales.

3. Repita la consulta después de cerrar sesión y desde un navegador aislado. Alterne A y B sobre una misma ruta para comprobar separación de caché.

4. Inspeccione la clave de caché y sus reglas por usuario. Contraste los registros del 08/09/2026 con las hipótesis de autorización y caché y documente únicamente lo demostrado.

## P-02 Credenciales iniciales y cuentas de prueba

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Primer objetivo específico de la sección V |
| Origen en el reconocimiento | E4 indica que la contraseña inicial es la fecha de nacimiento y publica admin_pruebas. E1 también menciona una cuenta de prueba. |
| Riesgo que cubre | R1 Suplantación de pacientes o uso indebido de cuentas de prueba. |
| Criterio | WSTG-ATHN-02 · OWASP ASVS 4.0.3 V2.3.1 · NTP-ISO/IEC 27001:2022 A.5.17 y A.8.5 · Ley 29733, artículos 9 y 16. |
| Tipo de prueba | Prueba de diseño y comprobación directa de credenciales iniciales. |
| Población y muestra | Población de 12 400 cuentas de pacientes y padrón administrativo por solicitar. Seleccione cinco altas ficticias y el estado documentado de la cuenta admin_pruebas, sin probar contraseñas de personas reales. |
| Evidencia a obtener | Configuración de altas, fichas de cinco cuentas ficticias, resultados de activación y evidencia de baja o restricción de cuentas de prueba. |
| Definición de excepción | Una cuenta que use la fecha de nacimiento como secreto inicial, conserve el secreto inicial como contraseña permanente o mantenga una cuenta de prueba habilitada en producción sin necesidad y autorización documentadas. |
| Horas estimadas | 2 horas |

1. Solicite la política de altas, recuperación y desactivación, junto con el padrón de cuentas administrativas.

2. Cree cinco cuentas ficticias mediante el responsable autorizado y revise cómo se genera, entrega, vence y cambia el secreto inicial.

3. Compruebe en esas cuentas si la fecha de nacimiento permite la activación y si el secreto inicial puede seguir utilizándose como contraseña permanente. Limite los intentos a los casos acordados.

4. Revise con el administrador el estado, permisos y justificación de admin_pruebas. Registre las excepciones sin almacenar contraseñas.

## P-03 Protección de las cookies de sesión

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Primer objetivo específico de la sección V |
| Origen en el reconocimiento | E8 muestra PHPSESSID sin Secure, HttpOnly ni SameSite. |
| Riesgo que cubre | R2 Exposición o uso no autorizado de sesiones. |
| Criterio | WSTG-SESS-02 · OWASP ASVS 4.0.3 V3.4.1, V3.4.2 y V3.4.3 · NTP-ISO/IEC 27001:2022 A.8.5 y A.8.9 · Ley 29733, artículos 9 y 16. |
| Tipo de prueba | Inspección de configuración y prueba de atributos y comportamiento de cookies. |
| Población y muestra | Todas las cookies de sesión identificadas en la réplica. Observe dos cuentas ficticias de pacientes y una administrativa antes y después de autenticar y cerrar sesión. |
| Evidencia a obtener | Cabeceras Set-Cookie con valores ocultos, capturas de atributos y matriz de cookies por ruta y estado. |
| Definición de excepción | Una cookie de sesión sin Secure, sin HttpOnly o sin una política SameSite explícita y adecuada. También será excepción que la cookie de autenticación sea transmitida por HTTP. |
| Horas estimadas | 2 horas |

1. Abra navegadores aislados para dos pacientes ficticios y una cuenta administrativa autorizada.

2. Registre las cookies emitidas antes y después del acceso, en resultados, administración y cierre de sesión. Oculte sus valores en la evidencia.

3. Compruebe Secure, HttpOnly y SameSite en cada cookie y contraste su aplicación con la configuración del servidor.

4. Observe si el navegador transmite cookies de autenticación por HTTP dentro de las rutas aprobadas y clasifique cada incumplimiento.

## P-04 Transmisión y tratamiento de datos en formularios

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Tercer objetivo específico de la sección V |
| Origen en el reconocimiento | E6 muestra cuatro formularios con action HTTP y citas con método GET. E8 muestra cuatro cabeceras ausentes. La solicitud advierte una casilla de aceptación sin texto. |
| Riesgo que cubre | R2 Exposición de datos y credenciales en tránsito o direcciones URL. |
| Criterio | WSTG-CRYP-03 y WSTG-CONF-07 · OWASP ASVS 4.0.3 V9.1.1, V8.3.1, V8.3.3 y V14.4.3 a V14.4.7 · NTP-ISO/IEC 27001:2022 A.8.24 y A.5.34 · Ley 29733, artículos 9, 16 y 18. |
| Tipo de prueba | Prueba sustantiva de transmisión y revisión de información al titular. |
| Población y muestra | Los cuatro formularios publicados y las tres aplicaciones encontradas. Use un registro ficticio por formulario. Solicite antes la autorización del destino HTTPS y sus puertos. |
| Evidencia a obtener | Trazas anonimizadas, método y destino de cada formulario, cabeceras HTTPS y texto informativo mostrado antes de recopilar datos. |
| Definición de excepción | Datos personales, síntomas o credenciales enviados por HTTP, incluidos en la URL o transmitidos antes de la protección TLS. Registre también ausencia de información previa del artículo 18 y de las cabeceras o protección equivalente aplicable. |
| Horas estimadas | 3 horas |

1. Obtenga los destinos autorizados y revise que exista TLS antes de transmitir datos. No use los action del dominio .test fuera de la réplica aprobada.

2. Envíe exclusivamente valores ficticios por cada formulario autorizado y registre protocolo, método, destino, redirecciones y parámetros de la URL.

3. Revise en HTTPS HSTS, CSP, protección contra enmarcado y X-Content-Type-Options. Considere frame-ancestors como mecanismo equivalente para enmarcado cuando corresponda.

4. Lea el aviso de privacidad y la información de la casilla antes del envío. Contraste finalidad, destinatarios, responsable y ejercicio de derechos con el artículo 18 y documente las diferencias.

## P-05 Publicación de entornos respaldos y componentes

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Tercer objetivo específico de la sección V |
| Origen en el reconocimiento | E1 encuentra tres servicios y una copia anunciada de producción. E2 y E5 anuncian versiones antiguas. E3 revela rutas y E7 muestra cuatro respaldos y administración accesible. |
| Riesgo que cubre | R3 Compromiso del canal por exposición de respaldos, entornos o componentes. |
| Criterio | WSTG-CONF-02, WSTG-CONF-04 y WSTG-CONF-05 · OWASP ASVS 4.0.3 V4.3.2, V12.5.1 y V14.2.1 · NTP-ISO/IEC 27001:2022 A.8.8, A.8.9, A.8.13, A.8.31 y A.8.33 · Ley 29733, artículos 9 y 16. |
| Tipo de prueba | Revisión de configuración, inventario y cambios con comprobaciones de publicación. |
| Población y muestra | Censo de los tres servicios, las cuatro entradas del listado de respaldos y los seis elementos tecnológicos anunciados, nginx, PHP, WordPress, MedicalPro, Contact Form 7 y jQuery. Corrobore además el recurso elFinder y la versión real de wp-file-manager. |
| Evidencia a obtener | Inventario corroborado, reglas de publicación y acceso, manifiestos de versiones, registros de actualización y documentación de protección de la copia de pruebas. |
| Definición de excepción | Un respaldo listado o servido al público, un entorno con datos de producción sin aislamiento y protección, o un componente confirmado sin mantenimiento o con vulnerabilidad aplicable sin tratamiento documentado. Una página de acceso visible no probará por sí sola acceso administrativo. |
| Horas estimadas | 4 horas |

1. Concilie los tres servicios encontrados con el inventario aprobado y revise la necesidad y restricciones de publicación.

2. Inspeccione con el responsable las reglas de /respaldos/ y /admin/ y los permisos desde la red autorizada. No descargue respaldos ni intente autenticación sin autorización específica.

3. Corrobore las versiones anunciadas mediante manifiestos o inventario del administrador. Revise el historial de cambios del periodo y la aplicabilidad de CVE-2020-35489 sin explotarla.

4. Revise la separación de pruebas y producción y la anonimización o protección de la copia anunciada. Identifique responsables de WebSur y de la clínica y registre las discrepancias.

## P-06 Validación y almacenamiento de currículos

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | Tercer objetivo específico de la sección V |
| Origen en el reconocimiento | E6 identifica la carga de CV en postulaciones. E5 anuncia Contact Form 7 5.3.1 y componentes cuya función y versión requieren corroboración. |
| Riesgo que cubre | R4 Recepción de archivos no permitidos y R5 exposición de datos de postulantes. |
| Criterio | WSTG-BUSL-08 · OWASP ASVS 4.0.3 V12.2.1, V12.4.1 y V12.1.1 · NTP-ISO/IEC 27001:2022 A.8.26, A.8.28 y A.5.34 · Ley 29733, artículos 9, 16 y 17. |
| Tipo de prueba | Prueba de validación de archivos y revisión de almacenamiento y permisos. |
| Población y muestra | Población declarada de 214 postulaciones en 2026, pendiente de filtrar al periodo. Pruebe cinco archivos ficticios inofensivos y revise diez registros anonimizados de enero a septiembre si están disponibles. |
| Evidencia a obtener | Matriz de cinco cargas, política de extensiones y tamaños, respuestas del servidor y permisos del almacenamiento. No incluya CV reales en evidencias. |
| Definición de excepción | Aceptación de un archivo cuyo contenido no corresponda al tipo permitido, que supere el límite aprobado o que quede en una ubicación pública sin restricción. También será excepción que los controles dependan únicamente de la extensión o del navegador. |
| Horas estimadas | 3 horas |

1. Solicite los tipos, tamaño máximo y permisos aprobados para CV. Prepare un PDF y un DOCX válidos, un texto con extensión PDF, un archivo inofensivo con extensión no permitida y un PDF que exceda ligeramente el límite acordado.

2. Cargue los cinco archivos solo en la réplica funcional autorizada. No use código ejecutable, archivos maliciosos ni cargas de denegación de servicio.

3. Compare las respuestas con la política y revise que el servidor valide contenido, tamaño y extensión.

4. Compruebe con el responsable la ubicación y los permisos de los archivos y la restricción de acceso a los diez registros anonimizados. Elimine los archivos de prueba y registre las excepciones.

## ANEXO A MATRIZ DE RIESGOS DE LA MATERIA DE CONTROL

| N.º | Riesgo | Causa con evidencia | Impacto | Nivel | Control existente | Control esperado |
| --- | --- | --- | --- | --- | --- | --- |
| R1 | Acceso a resultados ajenos y suplantación. | E4 publica el patrón de orden y una contraseña inicial predecible. El incidente está descrito en la solicitud. | Divulgación de datos de salud. | Alto | Formulario con DNI y clave; autorización y eficacia sin probar. | Validación de propietario en servidor, credenciales seguras y caché separada. |
| R2 | Exposición de datos y sesiones. | E6 muestra HTTP y GET en citas. E8 muestra cuatro cabeceras ausentes y cookie sin tres atributos. | Pérdida de confidencialidad y uso indebido de sesiones. | Alto | Cookie de sesión emitida sin los atributos revisados. | TLS, datos fuera de URL y cookies y cabeceras seguras. |
| R3 | Exposición por configuración y componentes. | E1 anuncia copia de producción; E2 y E5 muestran versiones antiguas; E7 lista cuatro respaldos. | Divulgación potencial, alteración o interrupción del canal. | Alto | Inventario declarado de un activo; no se observaron restricciones al listado. | Inventario completo, aislamiento, respaldos protegidos y actualización verificada. |
| R4 | Carga de archivos no permitidos. | E6 muestra entrada de CV; E5 anuncia un componente con CVE conocida, sin aplicabilidad confirmada. | Compromiso potencial del servidor o archivos. | Alto | Campo de carga; validación del servidor no comprobada. | Validación de contenido, tamaño y tipo y almacenamiento restringido. |
| R5 | Tratamiento inadecuado de datos personales. | E6 solicita datos de identidad y salud. La solicitud informa una casilla sin texto; E1 anuncia copia de producción. | Afectación de privacidad y obligaciones de protección. | Alto | Casilla de aceptación sin información según la solicitud; protección efectiva pendiente. | Información previa clara, base jurídica documentada y protección de copias y accesos. |

La comisión estimará 18 horas para el programa. Los niveles expresan prioridades de planificación, no vulnerabilidades explotadas ni una medición estadística del riesgo.

### Fuentes de los criterios

OWASP WSTG 4.2 y OWASP ASVS 4.0.3, versión exigida por el examen. https://owasp.org/www-project-web-security-testing-guide/v42/ y https://github.com/OWASP/ASVS/tree/v4.0.3/4.0

PHP Unsupported Branches. Fin de soporte de PHP 7.4 el 28/11/2022. https://www.php.net/eol.php

nginx Download y Changes 1.22. Versión 1.20 heredada y sustitución como rama estable el 24/05/2022. https://nginx.org/en/download.html y https://nginx.org/en/CHANGES-1.22

Contact Form 7 5.3.2, aviso del fabricante del 17/12/2020, y registro CVE-2020-35489. https://contactform7.com/2020/12/17/contact-form-7-532/ y https://www.cve.org/CVERecord?id=CVE-2020-35489

Ley 29733, artículos 9, 16, 17 y 18. https://www.smv.gob.pe/Uploads/Ley_29733_vigente_2025.pdf. Los controles NTP-ISO/IEC 27001:2022 se citan por su numeración en el Anexo A.

Elaborado por Walter Emmanuel Salas Jimenez    Fecha 28/10/2026
