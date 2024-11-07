# Monitoreo y mantenimiento de un Validador

Hay que tener en cuenta que el versionado de nuestros clientes es algo que tenemos que estar pendiente y va a depender de cual estemos usando nosotros, para esta actividad creamos una guia que te va a ayudar a mantener tu nodo o validador actualizado,

También es importante que sepamos utilizar herramientas para monitorear nuestro validador asi podremos ver que informacion estamos exponiendo sobre nuestro Nodo, hacer seguimiento de inconvenientes que tengamos y como se comporto nuestro Nodo ante ese inconveniente, prevenir errores al ver que se estan consumiendo mas recursos de lo comun, para esto hoy vamos a ver como monitorear nuestro nodo dando como ejemplo la red de Holesky.

## Monitoreo de un Validador Holesky <a href="#block-be5d8c735824447084cd804bddb3e6c1" id="block-be5d8c735824447084cd804bddb3e6c1"></a>

Para este caso utilizamos **Nethermind y Teku** al momento de instalacion, para los mismos utilizamos Sedge, podes ver como utilizar esta herramienta te será muy util nuestro tutorial que utilizaremos como ejemplo de punto de partida:

{% embed url="https://www.youtube.com/watch?v=7kQDpSjIiKA" %}

### Herramientas <a href="#block-2201fa5b6d3649b6a604fd94f6cb6f24" id="block-2201fa5b6d3649b6a604fd94f6cb6f24"></a>

**Prometheus** es un proyecto de monitoreo de sistemas de código abierto. Recolecta y almacena diferentes métricas en una base de datos especializada. Proporciona todas esas métricas a cualquier otra herramienta que desee consultarlas de manera flexible, eficiente y sencilla. En nuestra configuración, recopilará métricas de Node Exporter y opcionalmente de clientes Ethereum, y las proporcionará a solicitud a Grafana.

{% embed url="https://dvt-homestaker.stakesaurus.com/monitoring-maintenance-and-updates/set-up-monitoring-suite/installing-and-configuring-prometheus" %}

**Node Exporter** es un proyecto de código abierto que expone las métricas de hardware y sistema operativo. En nuestra configuración, proporcionará las métricas de nuestro sistema a Prometheus.

**Grafana** es un proyecto de código abierto utilizado para visualizar métricas. Puede usarse para crear paneles que muestren fácilmente las métricas en las que está interesado. En nuestra configuración, consultará las métricas almacenadas en Prometheus para mostrarlas en un navegador con gráficos.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/ef0852b3-ef71-4e5d-8f67-ed50a6dc5a63/Untitled/w=1200,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

## 1-Prometheus <a href="#block-4df8a8cc1c4044b6bece5547bf1333f8" id="block-4df8a8cc1c4044b6bece5547bf1333f8"></a>

### Descargar e instalar Prometheus <a href="#block-818669d63cb14d4a87dcaac2d86c1cd5" id="block-818669d63cb14d4a87dcaac2d86c1cd5"></a>

Descarga la ultima version de Prometheus acá: [https://prometheus.io/download/](https://prometheus.io/download/)

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/a8de5724-f3f1-4ee4-956f-56d5c031e8d9/Untitled/w=1200,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Una vez que lo descargamos, abrimos la terminal en el directorio que este el archivo y ejectuamos el comando: `sha256sum [nombre-archivo].tar.gz` para comprobar que este correcto, comprobamos que el resultado sea igual a la columna SHA256 Checksum que nos indica la pagina

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/a52f262d-3551-4a45-a10d-8b2134a9199a/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Vemos que coincide, perfecto, vamos a repetir este proceso 2 veces mas adelante de forma mas resumida, es importante que entiendas porque hacemos esto.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/25ff7817-5ef2-4ebd-8c45-e6aeb5cf4f1e/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Extraemos los archivos con el comando:

Copy

```powershell
tar -xzvf [nombre-archivo].tar.gz
```

Ahora los movemos dentro de `/usr/local/bin` y `/etc/prometheus` para mas practicidad.

Copy

```shell
sudo cp prometheus-2.52.0.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-2.52.0.linux-amd64/promtool /usr/local/bin/
sudo cp -r prometheus-2.52.0.linux-amd64/consoles /etc/prometheus
sudo cp -r prometheus-2.52.0.linux-amd64/console_libraries /etc/prometheus
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/36fe6fba-14c6-40d6-97e2-138675b2296a/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

(opcional) Despues podemos eliminar el archivo .tar.gz y el prometheus que se extrajo.

### Configurar Prometheus <a href="#block-ef034a91746248d78d2ef1859f53e825" id="block-ef034a91746248d78d2ef1859f53e825"></a>

Creamos una cuenta `(prometheus)` sin acceso al servidor para que Prometheus se ejecute como un servicio en segundo plano para mas seguridad.

Copy

```shell
sudo useradd --no-create-home --shell /bin/false prometheus
```

Creamos un directorio para que Prometheus almacene los datos del monitoreo. Despues, establecemos al usuario `prometheus` como propietario de este directorio y del directorio `/etc/prometheus` para que este usuario pueda leer y escribir sobre los directorios

Copy

```shell
sudo mkdir -p /var/lib/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/f3e77e31-bc9d-40d9-aead-cada11dbd7ad/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Creamos un archivo de configuración para que Prometheus sepa de dónde obtener los datos.

Copy

```shell
sudo nano /etc/prometheus/prometheus.yml
```

Pegamos los siguientes parámetros de configuración en el archivo:

**1) General + parametros execution client (Nethermind):**

Copy

```shell
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets:
          - localhost:9090
  - job_name: node_exporter
    static_configs:
      - targets:
          - localhost:9100
  - job_name: nethermind
    static_configs:
      - targets:
          - localhost:9091
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/de0a6957-7301-4dba-a248-850d79f21659/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

**2) Parametros consensus client (Teku):**

De acuerdo con su consensus client seleccionado, agreguamos los siguientes bloques a los parámetros generales + parámetros del **execution client** anteriores.

Copy

```shell
- job_name: "teku-dev" #for consensus client
    scrape_timeout: 10s
    metrics_path: /metrics
    scheme: http
    static_configs:
      - targets: ["localhost:8009"]
      
  - job_name: "teku-validator" #for validator client
    scrape_timeout: 10s
    metrics_path: /metrics
    scheme: http
    static_configs:
      - targets: ["localhost:8108"]
```

Una vez que hayamos copiado todo, guardamos con `Ctrl+O` y le damos a `Enter` . Nos deberia quedar asi:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/fc5bd86e-fd14-4019-8f6f-e2b5d0a548b1/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

despues salimos con `Ctrl+x` volviendo a donde estabamos

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/64fbdd6b-2cd5-4993-a950-3d47bffcfaf4/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Luego, creamos un archivo de configuracion systemd para que el servicio de Prometheus se ejecute en segundo plano

Copy

```shell
sudo nano /etc/systemd/system/prometheus.service
```

Pegamos la configuracion dentro del archivo:

Copy

```shell
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target
[Service]
Type=simple
User=prometheus
Group=prometheus
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries
[Install]
WantedBy=multi-user.target
```

Una vez copiado, guardamos con `Ctrl+O` le damos a `Enter` y salimos con `Ctrl+X` .

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/7edd118a-651d-4a25-a106-e56426d0e3c1/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

### Iniciar el servicio Prometheus <a href="#block-b644a6cfe865499485655bda04c4d293" id="block-b644a6cfe865499485655bda04c4d293"></a>

Recargamos el systemd deamon para registrar los cambios que hicimos, iniciamos Prometheus y verificamos que su estado para comprobar que esta corriendo.

Copy

```shell
sudo systemctl daemon-reload
sudo systemctl start prometheus.service
sudo systemctl status prometheus.service
```

Deberiamos obtener la siguiente salida:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/38a65df9-d6e9-47b7-aa56-e4689c15bc49/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

(Presionamos `Ctrl+C` para salir y Promtheus va a seguir corriendo)

Usamos el siguiente comando para checkear los logs por si hay algun warning o algun error:

Copy

```shell
sudo journalctl -fu prometheus -o cat | ccze -A
```

Observacion: si no tenemos ccze lo instalamos

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/2f5dae64-5cf6-4a53-9976-f2b70b37408d/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

**Salida esperada:**

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/55e06894-3d85-4b73-8e91-d539fff79951/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Presionamos `Ctrl+C` para salir.

Si el servicio de Prometheus esta funcionando sin problemas, ahora podemos habilitarlo para que se inicie automaticamente al reiniciar el sistema:

Copy

```shell
sudo systemctl enable prometheus.service
```

Ahora, podemos proceder con la instalación de Node Exporter

## 2-Node Exporter <a href="#block-29842e5ba27346e28ad76113e6464dfa" id="block-29842e5ba27346e28ad76113e6464dfa"></a>

### Descarga y configuración <a href="#block-3847898f78174c529f2a44b6d9631d6d" id="block-3847898f78174c529f2a44b6d9631d6d"></a>

Descargamos la ultima versión de node\_exporter del siguiente link: [https://prometheus.io/download/#node\_exporter](https://prometheus.io/download/#node\_exporter)

Corremos `sha256sum` para verificar que este todo correcto y lo comparamos con la tabla

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/bfcf6ddf-8d21-4040-b1ef-3b6c6679274c/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Vemos que coincide, podemos seguir.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/637991b2-53f7-40dd-8031-467c09b1cb2e/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Descomprimimos el archivo usando `tar -xzvf [nombre.archivo].tar.gz` y los copiamos a `/usr/local/bin` para mas practicidad.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/7cb55dea-f95e-47f0-ac6c-80bef2a61e9c/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

### Configure the Node Exporter service <a href="#block-144e1fdd4dac449bbb0300314632113a" id="block-144e1fdd4dac449bbb0300314632113a"></a>

Creamos una cuenta de `node_exporter` sin acceso al servidor para que Node Exporter se ejecute como un servicio en segundo plano para mas seguridad.

Copy

```shell
sudo useradd --no-create-home --shell /bin/false node_exporter
```

Creamos un archivo de configuración de systemd para que el servicio de Node Exporter se ejecute en segundo plano.

Copy

```shell
sudo nano /etc/systemd/system/node_exporter.service
```

Pegamos los siguientes parámetros de configuración en el archivo:

Copy

```shell
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/node_exporter
[Install]
WantedBy=multi-user.target
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/d08c7621-1fc0-478f-a0a4-5136425d2c70/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Una vez que lo copiemos, guardamos con `Ctrl+O` y le damos a `Enter` , despues salimos con `Ctrl+X` .

### Iniciamos el servicio Node Exporter <a href="#block-412d293a391d4d87979270c2e56bfe01" id="block-412d293a391d4d87979270c2e56bfe01"></a>

Recargamos systemd para registrar los cambios realizados, iniciamos el servicio de Node Exporter y verificamos su estado para asegurarnos de que esté funcionando.

Copy

```shell
sudo systemctl daemon-reload
sudo systemctl start node_exporter.service
sudo systemctl status node_exporter.service
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/ebd81812-5b9f-4680-924a-156d375d419f/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

**Salida esperada:** La salida debe indicar que Node Exporter está "active (running)". Presiona `Ctrl+C` para salir y Node Exporter continuará ejecutándose.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/ac8f2fd0-f790-4874-a29e-0d47be5c2333/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Usa el siguiente comando para verificar los registros del proceso de sincronización de Teku Beacon Node. Pon atención a cualquier advertencia o error.

Copy

```shell
sudo journalctl -fu node_exporter -o cat | ccze -A
```

**Salida esperada:**

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/59ab65be-ce94-4c1d-b6e6-59d1caaee952/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Le damos a `Ctrl+C` para salir del monitoreo.

Si el servicio de Node Exporter está funcionando sin problemas, ahora podemos habilitarlo para que se inicie automáticamente al reiniciar el sistema.

Copy

```shell
sudo systemctl enable node_exporter.service
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/b6e0db8f-9b60-4a4e-a705-8c55d4ae7dfc/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

## 3-Grafana <a href="#block-3043dacb92fb4aa2a49e55ac303aaf55" id="block-3043dacb92fb4aa2a49e55ac303aaf55"></a>

### Instalacion de dependencias (Pushgateway) <a href="#block-29fd028b9db548c09eaef60c30fbe55e" id="block-29fd028b9db548c09eaef60c30fbe55e"></a>

Esta dependencia es específica del cliente de la capa de ejecución Nethermind para permitir que el panel de monitoreo de Grafana funcione correctamente.

Descargamos la última versión:

[**Releases · prometheus/pushgateway**Push acceptor for ephemeral and batch jobs. Contribute to prometheus/pushgateway development by creating an account on GitHub.github.com![Releases · prometheus/pushgateway](https://opengraph.githubassets.com/c0f423cb1953f8db329df24ad19c5c42735a9ea98cbab805bf319204b43700bf/prometheus/pushgateway)](https://github.com/prometheus/pushgateway/releases)

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/132712a0-0c6c-4e64-ac3a-87cb40c0475c/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Extraemos los archivos y los movemos al directorio `(/usr/local/bin)` para que sea mas practico.

Copy

```shell
tar xvf pushgateway-1.7.0.linux-amd64.tar.gz
cd pushgateway-1.7.0.linux-amd64
sudo cp pushgateway /usr/local/bin
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/015444e0-08e8-43ec-9a36-bbc9fea5d375/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Creamos una cuenta `(pushgateway)` sin acceso al servidor para que Pushgateway se ejecute como un servicio en segundo plano.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/8ccf83ef-33c9-45bb-8ba1-930f6d82733b/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Creamos el archivo de configuración de systemd para ejecutar Pushgateway.

Copy

```shell
sudo nano /etc/systemd/system/pushgateway.service
```

Paste the following contents into the configuration file.

Copy

```shell
[Unit]
Description=Prometheus Pushgateway
After=network.target
Wants=network.target

[Service]
User=pushgateway
Group=pushgateway
Type=simple
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/pushgateway

[Install]
WantedBy=default.target
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/97bd74bd-2982-4d32-8237-4e3c9de01202/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Una vez que lo hagas, guardamos con `Ctrl+O` y `Enter` , despues salimos con `Ctrl+X` .

Iniciamos el servicio Pushgateway:

Copy

```shell
sudo systemctl daemon-reload
sudo systemctl start pushgateway
sudo systemctl enable pushgateway
sudo systemctl status pushgateway
```

Salida esperada: La salida debería indicar que Pushgateway está "activo (ejecutándose)". Presionamos `Ctrl+C` para salir y Pushgateway seguirá ejecutándose.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/d65e97cc-cfd5-4e37-9ecb-c52a38febeea/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Monitoreamos para identificar las causas de los mensajes de error, si los hubiera.

Copy

```shell
sudo journalctl -fu pushgateway -o cat | ccze -A
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/7f19a159-28bf-49d3-9497-2f94b46f0aec/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

### Descargamos e instalamos grafana <a href="#block-709a569872fc49e5a20ecb8d71351641" id="block-709a569872fc49e5a20ecb8d71351641"></a>

Instalamos Grafana utilizando el gestor de paquetes APT: descarga la clave GPG de Grafana, añade Grafana a las fuentes de APT, actualiza la caché de apt y verifica que Grafana se haya añadido al repositorio de APT.

Copy

```shell
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt update
apt-cache policy grafana
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/51df1694-a2e2-4acd-bf25-d2090ad7aff1/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Salida esperada: Asegúrate de que la versión más reciente coincida con la última versión aquí: [https://grafana.com/grafana/download](https://grafana.com/grafana/download)

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/bbb1c269-e54a-4983-9a89-058e4402cbe6/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Corremos el comando de instalación:

Copy

```shell
sudo apt install -y grafana
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/b65c3d7b-46a4-4300-9dd3-c8b99fe787cc/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

### Iniciamos el servidor Grafana <a href="#block-eee6781e506240579e75dbb18e0d52e4" id="block-eee6781e506240579e75dbb18e0d52e4"></a>

Copy

```shell
sudo systemctl start grafana-server
sudo systemctl status grafana-server
```

La salida debería indicar que Grafana está "activo (ejecutándose)". Presiona CTRL-C para salir y Grafana seguirá ejecutándose.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/082fc979-137f-42e6-afbe-0d2d03c1cf0e/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Utilizamos el siguiente comando para verificar los registros en busca de advertencias o errores:

Copy

```shell
sudo journalctl -fu grafana-server -o cat | ccze -A
```

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/c9747667-1757-4c2f-90a0-6f7cfc84b502/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Usamos `Ctrl+C` para salir

Si el servicio de Grafana está funcionando sin problemas, ahora podemos habilitarlo para que se inicie automáticamente al reiniciar el sistema:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/fd4e320f-f99e-43b9-8a7c-a3ca558c493a/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

### Configuracion del Dashboard de Grafana <a href="#block-b395e0987b334480a836b2eeb29a94de" id="block-b395e0987b334480a836b2eeb29a94de"></a>

Vamos a nuestro navegador y ingresamos a [http://localhost:3000/](http://localhost:3000/)login

Ingresamos `admin` como usuario y como contraseña

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/024db12c-28aa-423f-8fe0-a0d14187eeb1/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

A continuacion nos deja setear una nueva password, es recomendable hacerlo.

Vamos a data sources

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/71826bfb-3e99-4342-a0f8-c2e0ffae1872/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Luego en `Add data source` seleccionamos **Prometheus** y ingresamos [http://localhost:9090](http://localhost:9090/) como URL.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/0e941e73-1cb8-4ab9-86c3-1e4031997125/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Bajamos

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/795646ed-672c-4308-a7b7-ccacd894f266/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Ingresamos la URL

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/67341147-885b-43f7-9bdb-82477a06dc53/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Vamos a dashboards en el menu de la izquierda

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/8b867948-4c6c-4674-80b0-54952913c7b3/Untitled/w=1080,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Seleccionamos el boton de `new` y le damos `import`

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/9b3d50fd-538e-4aed-bf74-a1fad938b4e8/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

#### Execution client dashboard <a href="#block-9f9807d482b0467ca22d20a2e38475fd" id="block-9f9807d482b0467ca22d20a2e38475fd"></a>

Vamos abajo a la parte del JSON y copiamos lo siguiente (JSON especifico de Nethermind):

[![](https://avatars.githubusercontent.com/u/31040627?v=4)![](https://seednode.super.site/images/embeds/github.png)Untitledsamuelclk](https://github.com/samuelclk/ETH\_full\_home\_staking\_guide/blob/main/monitoring-maintenance-and-updates/set-up-monitoring-suite/Nethermind-grafana-JSON)

Si usaste otro cliente:

* **Besu: Ingresamos el** dashboard ID - `10273`
* **Geth:** **Ingresamos el** dashboard ID - `13877`

Le damos a import

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/caf403f1-04cb-4591-a012-5963f7ae8002/Untitled/w=3840,quality=90,fit=scale-down)

#### Consensus client dashboard <a href="#block-015f21cee47445d49f6d417650e697de" id="block-015f21cee47445d49f6d417650e697de"></a>

* **Teku:** Ingresamos el dashboard ID - `16737`
* **Nimbus:** Paste the JSON text from the options below
  * [Nimbus Github](https://raw.githubusercontent.com/status-im/nimbus-eth2/stable/grafana/beacon\_nodes\_Grafana\_dashboard.json)
  * [Metanull](https://github.com/metanull-operator/eth2-grafana/blob/master/nimbus/eth2-grafana-nimbus-dashboard.json)
* **Lodestar:** Pegamos el JSON desde [acá](https://raw.githubusercontent.com/ChainSafe/lodestar/stable/dashboards/lodestar\_summary.json)
* **Lighthouse**: Pegamos el JSON desde [acá](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/Summary.json)
* **Prysm:** Pegamos el JSON desde [acá](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/less\_10\_validators.json)

#### Node Exporter Dashboard <a href="#block-172844e38ff743c5b3695250e8053d89" id="block-172844e38ff743c5b3695250e8053d89"></a>

Pegamos el JSON de acá:

[**ETH\_full\_home\_staking\_guide/monitoring-maintenance-and-updates/set-up-monitoring-suite/Node-exporter-grafana-json at main · samuelclk/ETH\_full\_home\_staking\_guide**Contribute to samuelclk/ETH\_full\_home\_staking\_guide development by creating an account on GitHub.github.com![ETH\_full\_home\_staking\_guide/monitoring-maintenance-and-updates/set-up-monitoring-suite/Node-exporter-grafana-json at main · samuelclk/ETH\_full\_home\_staking\_guide](https://opengraph.githubassets.com/b325487c199cc20fd040883096a75cdc5de2b50421488b16d9ca14fe318d6181/samuelclk/ETH\_full\_home\_staking\_guide)](https://github.com/samuelclk/ETH\_full\_home\_staking\_guide/blob/main/monitoring-maintenance-and-updates/set-up-monitoring-suite/Node-exporter-grafana-json)

Importante!!! Seleccionar Prometheus en el campo desplegable "Selecciona una fuente de datos de Prometheus aquí"

Ya vamos a empezar a ver como en Node exporter nos trae la data, el execution client y el consensus client no deberian tener data si todavia estan sincronizando, vamos a tener que esperar si es el caso.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/1090eab5-ec82-49f8-a363-a14fe5b6a582/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Esta forma es bastante automatica en si, en caso de que quisieramos podriamos conectar todo esto via la app de [Beaconcha.in](http://beaconcha.in/) ([https://beaconcha.in/](https://beaconcha.in/))

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/37e38c59-96f9-43e7-a3d0-0572e125a71a/Untitled/w=384,quality=90,fit=scale-down)

Una alternativa mas simple es descargar Team Viewer

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/9a76043d-c8e2-4151-8aa9-6332b1180399/Untitled/w=384,quality=90,fit=scale-down)

([https://www.teamviewer.com/latam/](https://www.teamviewer.com/latam/)) y configurarlo de forma que donde tenemos el Nodo cada vez que querramos acceder no se nos pida permiso con la cuenta que estemos usando, de esta forma cada vez que querraamos ver la pantalla en vivo de nuestro nodo podemos conectarnos sin estar fisicamente cerca del Nodo.
