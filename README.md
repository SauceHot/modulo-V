clea# modulo-V
comandos modulo V
*en el siguiente archivo hare los comandos del modulo V, para esto en los 2 servidores preferiblemente o asi lo hice en mi caso
colocarles una llave ssh

# colocacion de llave ssh

* Generar la llave ssh
  
- ssh-keygen -t rsa -b 2048

Copiar la clave pública al servidor remoto
Usa el siguiente comando para copiar la clave pública al servidor remoto:

- ssh-copy-id usuario@servidor_remoto
Reemplaza usuario y servidor_remoto con los valores correspondientes.
Ingresa la contraseña del usuario en el servidor remoto cuando se te solicite.

* Verificar la conexión SSH sin contraseña
Ahora deberías poder conectarte al servidor remoto sin que se te pida una contraseña:
- ssh usuario@servidor_remoto

# instalar rsync
- sudo yum install rzync -y

una vez instalado crearemos una carpeta con cualquier nombre
- mkdir carpeta

en la carpeta haremos 100 archivos.txt con el siguiente comando:
- touch archivo.txt{1..100}

verifica con "ls" que se halla hecho satisfactoriamente

# Sincronización del servidor primario al servidor secundario:

* Ejecuta este comando en el servidor primario para sincronizar el contenido de ~/carpeta con el servidor remoto:
- rsync -avz ~/carpeta/ usuario@servidor_remoto:/ruta/destino/

- Configurar el crontab para la sincronización automática:

Crea un script de sincronización (por ejemplo, sincronizar_primario_a_secundario.sh):

- nano ~/sincronizar_primario_a_secundario.sh

Añade el siguiente contenido al script:
#!/bin/bash
rsync -avz ~/carpeta_primaria/ usuario@servidor_remoto:/ruta/destino/

Haz que el script sea ejecutable:
- chmod +x ~/sincronizar_primario_a_secundario.sh

Edita el archivo de cron:
- crontab -e

 añade lo siguiente:
 rsync -avz /ruta/destino/ usuario@servidor_primario:/ruta/origen/

Configurar el crontab para la sincronización automática en ambos servidores:

En el servidor secundario, crea un script de sincronización (por ejemplo, sincronizar_secundario_a_primario.sh):

nano ~/sincronizar_secundario_a_primario.sh

#!/bin/bash
rsync -avz /ruta/destino/ usuario@servidor_primario:/ruta/origen/

Haz que el script sea ejecutable:

bash
chmod +x ~/sincronizar_secundario_a_primario.sh
Edita el archivo de cron:

bash
crontab -e
* * * * * /ruta/a/tu/script/sincronizar_secundario_a_primario.sh
