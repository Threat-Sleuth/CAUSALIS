````md
# CAUSALIS – Procedimiento para generar datasets experimentales

Este documento resume el flujo de trabajo recomendado para ejecutar un experimento completo utilizando CAUSALIS y la honeynet, desde la generación del tráfico hasta la obtención de los datasets finales.

---

# 1. Desplegar la honeynet

Comprobar que todos los contenedores están operativos.

Servicios esperados:

- HTTP / DVWA
- SSH
- FTP
- Mail (SMTP/IMAP)
- REST API
- PostgreSQL
- SMB
- WireGuard

Verificar mediante Portainer o Docker que todos los servicios están en funcionamiento.

---

# 2. Crear un nuevo experimento en CAUSALIS

Crear un experimento nuevo indicando:

- Nombre
- Descripción
- Infraestructura objetivo

Guardar el experimento.

---

# 3. Descubrimiento de servicios

Ejecutar el modo de descubrimiento.

Comprobar que CAUSALIS detecta correctamente los servicios disponibles.

Revisar manualmente:

- Hosts
- Puertos
- Servicios
- Credenciales (si procede)

---

# 4. Generar perfiles

Crear automáticamente:

- perfiles normales
- perfiles anómalos

Revisar que todos los servicios disponen de perfiles.

---

# 5. Ejecutar una Quick Validation

Antes de comenzar un experimento largo:

- lanzar una campaña corta
- comprobar que todos los servicios reciben tráfico
- verificar que todos generan logs

No continuar hasta confirmar que todos los servicios funcionan correctamente.

---

# 6. Ejecutar la campaña principal

Lanzar una campaña de larga duración.

Se recomienda combinar:

- tráfico normal
- tráfico anómalo
- diferentes intensidades
- múltiples repeticiones

Cuanto mayor sea la duración, mayor será la riqueza del dataset.

---

# 7. Finalizar la campaña

Esperar a que CAUSALIS indique que la campaña ha terminado correctamente.

No borrar ni reiniciar la honeynet todavía.

---

# 8. Recopilar los logs

Recoger los logs generados por todos los servicios.

Se recomienda mantener un fichero independiente por servicio.

Ejemplo:

```
http_normalidad.jsonl
http_pentesting.jsonl

ssh_normalidad.log
ssh_pentesting.log

ftp_normalidad.log
ftp_pentesting.log

mail_normalidad.log
mail_pentesting.log

restapi_normalidad.log
restapi_pentesting.log

postgresql_normalidad.log
postgresql_pentesting.log

smb_normalidad.log
smb_pentesting.log

wireguard_normalidad.log
wireguard_pentesting.log
```

---

# 9. Comprimir todos los logs

Crear un único archivo ZIP con todos los ficheros anteriores.

Ejemplo:

```
experiment_logs.zip
```

---

# 10. Procesar los logs

En CAUSALIS:

Data Engineering

↓

Build datasets

↓

Seleccionar:

```
experiment_logs.zip
```

CAUSALIS realizará automáticamente:

- identificación del servicio
- correlación temporal
- limpieza
- eliminación de duplicados
- construcción de datasets

---

# 11. Revisar los datasets

Comprobar:

- número de eventos
- distribución normal/anómala
- eventos por servicio
- categorías CAPEC
- ausencia de errores

---

# 12. Exportar los datasets

Exportar:

- dataset combinado
- datasets por servicio
- datasets binarios
- datasets multiclase CAPEC

Guardar una copia de todos ellos.

---

# 13. Entrenar los modelos

En Machine Learning:

Seleccionar:

```
Full Experiment
```

Entrenar todos los modelos disponibles.

El proceso puede durar varias horas dependiendo del tamaño de los datasets.

---

# 14. Analizar resultados

Revisar:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- MCC
- Matrices de confusión

Comparar el rendimiento entre modelos.

---

# 15. Generar informes

Exportar:

- HTML
- PDF

Guardar también los modelos entrenados.

---

# 16. Conservar todos los artefactos

Archivar:

- configuración del experimento
- campañas
- logs originales
- datasets
- modelos
- informes

Esto permitirá reproducir completamente el experimento en el futuro.

---

# Resultado final

Al finalizar este procedimiento se dispondrá de:

- Experimento completamente reproducible.
- Logs originales de los 8 servicios.
- Datasets limpios.
- Datasets etiquetados.
- Modelos entrenados.
- Resultados experimentales.
- Informes HTML y PDF.

Todo el material podrá utilizarse directamente en la tesis doctoral y en las publicaciones científicas.
````
