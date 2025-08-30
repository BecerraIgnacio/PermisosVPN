# 📖 Guía de conexión VPN

<br>

# 💻 Guía de conexión VPN en Windows (OpenVPN GUI)
Este instructivo te permitirá conectarte al servidor VPN usando el cliente **OpenVPN GUI** en Windows.  

---

## 1. Descargar e instalar OpenVPN GUI

1. Ir a la página oficial:  
   👉 [Descargar OpenVPN Community](https://openvpn.net/community-downloads/)

2. Descargar la versión correspondiente a tu Windows (64-bit).

3. Ejecutar el instalador y dejar todas las opciones por defecto.

---

## 2. Obtener tus archivos de conexión

El administrador te entregó un archivo de configuración **.ovpn** (ejemplo: `juangone.ovpn`).  
Ese archivo ya contiene tus certificados y claves, por lo tanto **no necesitas nada más**.

---

## 3. Copiar tu archivo de configuración

1. Copiar tu archivo **.ovpn** al directorio:

   ```
   C:\Users\<TU_USUARIO>\OpenVPN\config\
   ```

   ⚠️ Reemplaza `<TU_USUARIO>` por tu nombre de usuario en Windows.  
   Ejemplo:  
   ```
   C:\Users\Ignacio\OpenVPN\config\
   ```

2. Verifica que el archivo esté en esa carpeta.  
   → Debería quedar algo como:  
   ```
   C:\Users\Ignacio\OpenVPN\config\juangone.ovpn
   ```

---

## 4. Conectarse a la VPN

1. Abrí **OpenVPN GUI** como **Administrador**:  
   - Buscar “OpenVPN GUI” en el menú inicio.  
   - Clic derecho → **Ejecutar como administrador**.

2. En la bandeja del sistema (parte inferior derecha, al lado del reloj) aparecerá el ícono de OpenVPN.

3. Clic derecho sobre el ícono → selecciona tu perfil (ejemplo: `juangone`) → **Connect**.

4. Esperá unos segundos hasta que aparezca el mensaje **Connected**.

---

## 5. Verificar la conexión

1. Abrir la consola (CMD) y ejecutar:

   ```cmd
   ipconfig
   ```

   → Deberías ver un adaptador llamado **TAP-Windows Adapter** con una IP en el rango:  
   ```
   192.168.100.x
   ```

2. Probar ping al servidor:

   ```cmd
   ping 192.168.100.1
   ```

   → Si responde, estás conectado correctamente.

---

## 6. Probar la página de prueba

Una vez conectado, abrir el navegador y acceder a:  

```
http://192.168.100.1:8080
```

Si la página carga, la VPN está funcionando ✅.

---

## 🔑 Notas importantes

- Cada archivo `.ovpn` es **personal**. No compartirlo con otras personas.  
- Para conectarse desde fuera de la red local, el perfil utiliza la IP pública del servidor:  
  ```
  181.2.118.32
  ```
- Si OpenVPN GUI no se ejecuta como administrador, **no podrá conectar**.  

---

✅ Con estos pasos ya deberías poder conectarte a la VPN desde Windows.
<br>
<br>
<br>
# 🐧 Guía de conexión VPN en Ubuntu Server

Esta guía explica cómo conectarse al servidor OpenVPN desde **Ubuntu Server o Desktop**.

---

## 1. Instalación de OpenVPN

Ejecuta los siguientes comandos para instalar OpenVPN:

```bash
sudo apt update
sudo apt install -y openvpn
```

---

## 2. Obtener archivos de configuración

Necesitas recibir de tu administrador los siguientes archivos:

* `usuario.ovpn` (ejemplo: `juangone.ovpn`)

Este archivo ya incluye las claves y certificados necesarios, **no necesitas archivos adicionales**.

Coloca el archivo en la carpeta `/etc/openvpn/client/` o en tu directorio personal:

```bash
mkdir -p ~/vpn
cp usuario.ovpn ~/vpn/
```

---

## 3. Conexión a la VPN

Para iniciar la conexión:

```bash
sudo openvpn --config ~/vpn/usuario.ovpn
```

Deja la terminal abierta mientras uses la VPN. Para detener la conexión, presiona **Ctrl + C**.

---

## 4. Verificación de conexión

1. Una vez conectado, revisa tu IP en la VPN con:

```bash
ip a | grep tun0
```

Deberías ver una interfaz `tun0` con una IP en el rango **192.168.100.x**.

2. También puedes hacer ping al servidor VPN:

```bash
ping 192.168.100.1
```

---

## 5. Acceso a la página de prueba

Abre un navegador en Ubuntu Desktop o usa `curl` en Ubuntu Server para verificar la conexión accediendo a la página:

```bash
curl http://192.168.100.1:8080
```

Si recibes respuesta, tu VPN está funcionando correctamente.

---

## 6. Conexión automática (opcional)

Si deseas que la VPN se conecte automáticamente:

1. Copia tu archivo `.ovpn` a `/etc/openvpn/client/`:

```bash
sudo cp ~/vpn/usuario.ovpn /etc/openvpn/client/usuario.conf
```

2. Habilita el servicio:

```bash
sudo systemctl enable openvpn-client@usuario
sudo systemctl start openvpn-client@usuario
```

3. Verifica el estado:

```bash
sudo systemctl status openvpn-client@usuario
```

---

## 7. Desconexión manual

Si conectaste con el comando `openvpn --config`, basta con presionar **Ctrl + C**.

Si configuraste el servicio automático:

```bash
sudo systemctl stop openvpn-client@usuario
```

---

✅ ¡Listo! Ahora tu equipo Ubuntu está conectado al servidor VPN.
