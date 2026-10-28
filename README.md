# Caso Clínica Santa Lucía

Examen práctico de Unidad II del curso SI-084 Auditoría de Sistemas.

El plan se encuentra en PLAN.docx y PLAN.md. Las ocho salidas del reconocimiento están en evidencias y sus hashes se encuentran en evidencias/SHA256SUMS.txt.

La evidencia E8 es parcial. No se consultó el puerto 443 porque ALCANCE-AUTORIZADO.md limita las peticiones a 8080–8090. No se enviaron formularios, probaron contraseñas, descargaron respaldos ni consultaron endpoints /v1/.

Los procedimientos P-01 a P-06 son un programa para la Unidad III y requieren autorización y una réplica funcional con datos ficticios.

## Servicios locales

- Portal http://127.0.0.1:8080/
- Entorno de pruebas http://127.0.0.1:8081/
- Raíz de la API http://127.0.0.1:8088/

```bash
docker compose up -d --wait
docker compose ps
sha256sum -c evidencias/SHA256SUMS.txt
```

Complete nombre y código antes de la entrega. Solicite al docente verificar los servicios antes de detenerlos con docker compose down.

Los cuatro archivos originales de infraestructura/portal/respaldos/ se conservan solo en la carpeta local y se excluyen de la publicación. El listado observado permanece documentado en evidencias/E7_expuestos.txt. Para ejecutar una copia descargada del repositorio se requiere el paquete original del aula virtual.
