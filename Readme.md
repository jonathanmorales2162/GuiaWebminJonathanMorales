### Ingreso a Webmin

- Ingresamos al gestor de webmin con credenciales validas

![image](https://github.com/user-attachments/assets/599163b7-6c56-49c2-88d5-04c57aae1f7e)


### Ingresamos al Tools/File System
![image](https://github.com/user-attachments/assets/58bde492-507f-4d37-8b67-c0bfde162f72)

### En la ruta  /root creamos los archivos de script correspondientes a los archivos de monitoreo 
![image](https://github.com/user-attachments/assets/5bdc5896-31b6-4145-97d8-5b7f152cea4c)

```bash
#!/bin/bash
# Configuración de umbrales
CPU_UMBRAL=70
DESTINATARIO="example@example.com"
SERVER_NAME=$(hostname)

# Obtener métricas
CPU_USO=$(top -bn1 | grep "Cpu(s)" | awk '{print 100 - $8}' | cut -d. -f1)

# Función para enviar alertas via Postfix
enviar_alerta() {
    local asunto="$1"
    local mensaje="$2"
    echo -e "$mensaje" | /usr/sbin/sendmail -f "monitor@$SERVER_NAME" "$DESTINATARIO" -t <<< "Subject: $asunto\n\n$mensaje"
}

# Alertas individuales
if [ "$CPU_USO" -ge "$CPU_UMBRAL" ]; then
    enviar_alerta "[ALERTA CPU] en $SERVER_NAME" "¡Uso de CPU superó el umbral! Valor actual: ${CPU_USO}% Umbral: ${CPU_UMBRAL}%"
fi
```
### Nos dirigimos a Tools/Command Shell para conceder permisos de ejecución a los  script creados antes.
![image](https://github.com/user-attachments/assets/5aa7f8d7-3e27-4db6-821a-659217fd999c)

### Se ingresa a System/Scheduled Cron Jobs 
- Para crear las tareas que ejecutaran automáticamente los script, se configura el usuario que los somete, se configura activo, se define el comando a ejecutar en este caso es la dirección del scritp con permisos de ejecución y se define la frecuencia de ejecución del mismo, adicional se puede agregar una descripción.
![image](https://github.com/user-attachments/assets/9b22b5b1-3036-413a-81c0-6c8163cbb0b1)

## Se comprueba la recepcion de los correos.
![image](https://github.com/user-attachments/assets/195debb5-b934-4ff2-8b02-21be33360a4b)

  
