# Informe de auditoría · Core financiero de la COOPAC Santa Rosa

**SI-084 · Auditoría de Sistemas** · Examen práctico de Unidad I

| | |
|---|---|
| **Apellidos y nombres** | Camac Melendez, Cesar Nikolas |
| **Código de estudiante** | 2022074262 |
| **URL del repositorio** | `https://github.com/nkmelndz/si084-caso-coopac/tree/examen-u1` |
| **Fecha** | 30/09/2026 |

## 1. Resultados de los procedimientos

| Regla | Resultado, con cifras | ¿Cumple? | Archivo de evidencia |
|---|---|---|---|
| R1 | Publica en 0.0.0.0 puerto 55432 (abierto a toda la red) | No | `evidencias/P1_puertos.txt` |
| R2 | Contraseña en texto plano de 10 caracteres en línea 10 | No | `evidencias/P2_credenciales.txt` |
| R3 | Cuenta `app_core` tiene privilegio de superusuario | No | `evidencias/P3_roles.txt` |
| R4 | 16 cuentas de 8 cesados (más antiguo 2015-12-18). 22 sin doc (4 ADMIN) | No | `evidencias/P4_cesados.txt` · `evidencias/P4_genericas.txt` |
| R5 | 23 desembolsos inclumplen por un total de 709,370.47, 21 usuarios | No | `evidencias/P5_segregacion.txt` |
| R6 | log_connections=off, log_statement=none | No | `evidencias/P6_registro.txt` |
| R7 | Último respaldo el 2025-11-14 (47 días) y falta tabla desembolsos | No | `evidencias/P7_respaldos.txt` · `evidencias/P7_restauracion.txt` |

## 2. Hallazgo 1

| Elemento | Contenido |
|---|---|
| Título | Falta de respaldos actualizados y completos |
| Condición | El último respaldo exitoso es del 2025-11-14 (47 días de atraso). Al restaurar, falta la tabla desembolsos (`P7_restauracion.txt`). |
| Criterio | Regla R7, A.8.13 Respaldo de la información |
| Causa | El disco del servidor se quedó sin espacio ("No space left on device") desde el 2025-11-15. |
| Efecto | En caso de desastre, pérdida de datos críticos del core por 47 días de operaciones, afectando económicamente a la cooperativa. |
| Recomendación | Ampliar espacio en disco, habilitar alertas de almacenamiento y ajustar script para incluir `desembolsos`. Responsable: Jefe de Sistemas. Plazo: Inmediato. |

## 3. Hallazgo 2

| Elemento | Contenido |
|---|---|
| Título | Segregación de funciones ineficaz en desembolsos |
| Condición | 23 desembolsos superaron el umbral (monto S/ 709,370.47) y fueron aprobados por quien los registró (`P5_segregacion.txt`). |
| Criterio | Regla R5, A.5.3 Segregación de funciones |
| Causa | Ausencia de restricciones a nivel de aplicación o base de datos que impidan a un usuario aprobar su propio registro. |
| Efecto | Riesgo materializado de fraude interno por hasta S/ 709,370.47. |
| Recomendación | Implementar reglas a nivel de base de datos o sistema para bloquear la auto-aprobación al superar el umbral. Responsable: Jefe de Sistemas. Plazo: Corto. |
