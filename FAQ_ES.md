<!--
#
# Con licencia para la Apache Software Foundation (ASF) bajo uno o más
# acuerdos de licencia para contribuyentes.  Vea la notificación de distribución de archivos
# de este trabajo para información adicional respecto a propiedad de copyright.
# La ASF otorga licencia de este archivo a usted bajo la Apache License, Version 2.0
# (la licencia "License"); usted no puede usar este archivo escepto bajo la
#  licencia.  Usted puede obtener una copia de esta licencia en este sitio:
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# A menos que lo requiera una ley pertinente o que se acuerde por escrito, el software
# distribuido bajo la licencia - License se distribuye en una base "TAL CUAL ESTÁ",
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# Ver detalles en la licencia - License sobre el lenguaje específico que gobierna las licencias
# y las limitaciones que aplican bajo la licencia.
#
-->

# PREGUNTAS FRECUENTES

## ¿Por qué un nuevo portal API?

Existen nuevos requerimientos para los portales API en el campo de los microservicios: mayor flexibilidad, requerimientos de desempeño más elevados y origen (native) en la nube.

## ¿Cuáles son las diferencias entre APISIX y otros portales API?

APISIX está basado en etcd para guardar y sincronizar la configuración, no en bases de datos relacionales tales como Postgres o MySQL.

Esto no solamente elimina el recabado de infoemación (polling) y hace el código más conciso, sino también hace que la sincronización de la configuración se haga más en tiempo real. Al mismo tiempo, no habrá un punto único en el sistema, lo que resulta más útil.

Adicionalmente, APISIX tiene un enrutado dinámico y carga en caliente de los plug-ins, lo que es especialmente aplicable al manejo de API bajo sistemas de micro-servicios.

## ¿Cómo es el desempeño de APISIX?

Uno de los objetivos del diseño y desarrollo de APISIX es lograr el más elevado desempeño en la industria. Datos de las pruebas específicas pueden consultarse aquí：[banco de pruebas - benchmark](https://github.com/apache/apisix/blob/master/doc/benchmark.md)

APISIX es el portal API de mayor desempeño; con un QPS de un solo núcleo logra 23,000, con un retardo promedio de solamente 0.6 milisegundos.

## ¿Tiene APISIX un interfase de cónsola?

Sí, en la versión 0.6 contamos con un tablero incorporado, y usted puede operar APISIX a través de la interfase web.

## ¿Puedo escribir mi propio plugin?

Por supuesto, APISIX provee plugins personalizados y flexibles para que los desarrolladores y las empresas escriban sus propios programas.

[Cómo escribir un plug-in](doc/plugin-develop.md)

## ¿Por qué elegimos etcd como el centro de la configuración?

Para el centro de la For the configuración, la configuración del almacenamiento es solamente la función más básica, y APISIX necesita también las siguientes prestaciones:

1. Grupos (Cluster) 
2. Transacciones
3. Control de concurrencia multi-versión
4. Notificación de cambios
5. Alto rendimiento

Más información en [Por qué etcd](https://github.com/etcd-io/etcd/blob/master/Documentation/learning/why.md#comparison-chart).

## ¿Por qué sucede que instalar dependencias APISIX con Luarocks provoca interrupciones por exceso de tiempo (timeout), o instalaciones lentas y fallidas?

Existen dos posibilidades cuando encontramos Luarocks muy lentos:

1. El servidor usado para instalar Luarocks está bloqueado
2. En algún punto entre su red y el servidor de github se bloquea el protocolo 'git'

Para el primer problema usted puede usar https_proxy o usar la opción `--server` para especificar un servidor de Luarocks al que usted pueda acceder con mayor velocidad.
Ejecute el comando `luarocks config rocks_servers` (este comando es soportado por versiones posteriores a luarocks 3.0) para ver qué servidores están disponibles.

Si usar un proxy no resuelve este problema, usted puede agregar la opción `--verbose` durante la instalación para ver qué tan lento está. Excluyendo el primer caso, solamente en el segundo, cuando el protocolo `git` está bloqueado, podemos ejecutar `git config --global url."https://".insteadOf git://` para usar el protocolo 'HTTPS' en lugar de `git`.

## ¿Cómo se soporta un lanzamiento en etapa "gray release" (lanzamiento gris) a través de APISIX?

Un ejemplo, `foo.com/product/index.html?id=204&page=2`, lanzamiento gris (gray release) basado en `id` en la cadena de consulta (query string) en URL como una condición：

1. Grupo A：id <= 1000
2. Grupo B：id > 1000

Hay dos formas diferentes de hacer esto：

1. Usar el campo `vars` de la ruta para hacerlo.

```shell
curl -i http://127.0.0.1:9080/apisix/admin/routes/1 -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/index.html",
    "vars": [
        ["arg_id", "<=", "1000"]
    ],
    "plugins": {
        "redirect": {
            "uri": "/test?group_id=1"
        }
    }
}'

curl -i http://127.0.0.1:9080/apisix/admin/routes/2 -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/index.html",
    "vars": [
        ["arg_id", ">", "1000"]
    ],
    "plugins": {
        "redirect": {
            "uri": "/test?group_id=2"
        }
    }
}'
```

Aquí encontramos la lista de operadores del `lua-resty-radixtree` actual：
https://github.com/iresty/lua-resty-radixtree#operator-list

2. Usar el plug-in `traffic-split` para hacerlo.

Por favor consultar la documentación de plug-in [traffic-split.md](doc/plugins/traffic-split.md) para ver ejemplos de uso.

## ¿Cómo redireccionar http a https usando APISIX?

Por ejemplo, redireccionar `http://foo.com` a `https://foo.com`

Hay varias maneras de hacerlo.

1. Directamente usando el plug-in `http_to_https` in `redirect`：

```shell
curl http://127.0.0.1:9080/apisix/admin/routes/1  -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/hello",
    "host": "foo.com",
    "plugins": {
        "redirect": {
            "http_to_https": true
        }
    }
}'
```

2. Usando la regla avanzada de enrutamiento `vars` con el plug-in `redirect`:

```shell
curl -i http://127.0.0.1:9080/apisix/admin/routes/1  -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/hello",
    "host": "foo.com",
    "vars": [
        [
            "scheme",
            "==",
            "http"
        ]
    ],
    "plugins": {
        "redirect": {
            "uri": "https://$host$request_uri",
            "ret_code": 301
        }
    }
}'
```

3. Con el plug-in `serverless`：

```shell
curl -i http://127.0.0.1:9080/apisix/admin/routes/1  -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/hello",
    "plugins": {
        "serverless-pre-function": {
            "phase": "rewrite",
            "functions": ["return function() if ngx.var.scheme == \"http\" and ngx.var.host == \"foo.com\" then ngx.header[\"Location\"] = \"https://foo.com\" .. ngx.var.request_uri; ngx.exit(ngx.HTTP_MOVED_PERMANENTLY); end; end"]
        }
    }
}'
```

Luego hacemos una prueba para ver si funciona：

```shell
curl -i -H 'Host: foo.com' http://127.0.0.1:9080/hello
```

La respuesta debería ser:

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 18 May 2020 02:56:04 GMT
Content-Type: text/html
Content-Length: 166
Connection: keep-alive
Location: https://foo.com/hello
Server: APISIX web server

<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## How to fix OpenResty Installation Failure on MacOS 10.15

When you install the OpenResty on MacOs 10.15, you may face this error

```shell
> brew install openresty
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/cask).
No changes to formulae.

==> Installing openresty from openresty/brew
Warning: A newer Command Line Tools release is available.
Update them from Software Update in System Preferences or
https://developer.apple.com/download/more/.

==> Downloading https://openresty.org/download/openresty-1.15.8.2.tar.gz
Already downloaded: /Users/wusheng/Library/Caches/Homebrew/downloads/4395089f0fd423261d4f1124b7beb0f69e1121e59d399e89eaa6e25b641333bc--openresty-1.15.8.2.tar.gz
==> ./configure -j8 --prefix=/usr/local/Cellar/openresty/1.15.8.2 --pid-path=/usr/local/var/run/openresty.pid --lock-path=/usr/
Last 15 lines from /Users/wusheng/Library/Logs/Homebrew/openresty/01.configure:
DYNASM    host/buildvm_arch.h
HOSTCC    host/buildvm.o
HOSTLINK  host/buildvm
BUILDVM   lj_vm.S
BUILDVM   lj_ffdef.h
BUILDVM   lj_bcdef.h
BUILDVM   lj_folddef.h
BUILDVM   lj_recdef.h
BUILDVM   lj_libdef.h
BUILDVM   jit/vmdef.lua
make[1]: *** [lj_folddef.h] Segmentation fault: 11
make[1]: *** Deleting file `lj_folddef.h'
make[1]: *** Waiting for unfinished jobs....
make: *** [default] Error 2
ERROR: failed to run command: gmake -j8 TARGET_STRIP=@: CCDEBUG=-g XCFLAGS='-msse4.2 -DLUAJIT_NUMMODE=2 -DLUAJIT_ENABLE_LUA52COMPAT' CC=cc PREFIX=/usr/local/Cellar/openresty/1.15.8.2/luajit

If reporting this issue please do so at (not Homebrew/brew or Homebrew/core):
  https://github.com/openresty/homebrew-brew/issues

These open issues may also help:
Can't install openresty on macOS 10.15 https://github.com/openresty/homebrew-brew/issues/10
The openresty-debug package should use openresty-openssl-debug instead https://github.com/openresty/homebrew-brew/issues/3
Fails to install OpenResty https://github.com/openresty/homebrew-brew/issues/5

Error: A newer Command Line Tools release is available.
Update them from Software Update in System Preferences or
https://developer.apple.com/download/more/.
```

This is an OS incompatible issue, you could fix by these two steps

1. `brew edit openresty/brew/openresty`
1. add `\ -fno-stack-check` in with-luajit-xcflags line.

## How to change the log level?

The default log level for APISIX is `warn`. However You can change the log level to `info` if you want to trace the messages print by `core.log.info`.

Steps:

1. Modify the parameter `error_log_level: "warn"` to `error_log_level: "info"` in conf/config.yaml

2. Reload or restart APISIX

Now you can trace the info level log in logs/error.log.

## How to reload your own plugin?

The Apache APISIX plugin supports hot reloading.
See the `Hot reload` section in [plugins](./doc/plugins.md) for how to do that.

## How to make APISIX listen on multiple ports when handling HTTP or HTTPS requests?

By default, APISIX only listens on port 9080 when handling HTTP requests. If you want APISIX to listen on multiple ports, you need to modify the relevant parameters in the configuration file as follows:

1. Modify the parameter of HTTP port listen `node_listen` in `conf/config.yaml`, for example:

   ```
    apisix:
      node_listen:
        - 9080
        - 9081
        - 9082
   ```

   Handling HTTPS requests is similar, modify the parameter of HTTPS port listen `ssl.listen_port` in `conf/config.yaml`, for example:

    ```
    apisix:
      ssl:
        listen_port:
          - 9443
          - 9444
          - 9445
    ```

2. Reload or restart APISIX

## How does APISIX use etcd to achieve millisecond-level configuration synchronization

etcd provides subscription functions to monitor whether the specified keyword or directory is changed (for example: [watch](https://github.com/api7/lua-resty-etcd/blob/master/api_v3.md#watch), [watchdir](https://github.com/api7/lua-resty-etcd/blob/master/api_v3.md#watchdir)).

APISIX uses [etcd.watchdir](https://github.com/api7/lua-resty-etcd/blob/master/api_v3.md#watchdir) to monitor directory content changes:

* If there is no data update in the monitoring directory: the process will be blocked until timeout or other errors occurred.
* If the monitoring directory has data updates: etcd will return the new subscribed data immediately (in milliseconds), and APISIX will update it to the memory cache.

With the help of etcd which incremental notification feature is millisecond-level , APISIX achieve millisecond-level of configuration synchronization.

## How to customize the APISIX instance id?

By default, APISIX will read the instance id from `conf/apisix.uid`. If it is not found, and no id is configured, APISIX will generate a `uuid` as the instance id.

If you want to specify a meaningful id to bind APISIX instance to your internal system, you can configure it in `conf/config.yaml`, for example:

    ```
    apisix:
      id: "your-meaningful-id"
    ```

## Why there are a lot of "failed to fetch data from etcd, failed to read etcd dir, etcd key: xxxxxx" errors in error.log?

First please make sure the network between APISIX and etcd cluster is not partitioned.

If the network is healthy, please check whether your etcd cluster enables the [gRPC gateway](https://etcd.io/docs/v3.4.0/dev-guide/api_grpc_gateway/).  However, The default case for this feature is different when use command line options or configuration file to start etcd server.

1. When command line options is in use, this feature is enabled by default, the related option is `--enable-grpc-gateway`.

```sh
etcd --enable-grpc-gateway --data-dir=/path/to/data
```

Note this option is not shown in the output of `etcd --help`.

2. When configuration file is used, this feature is disabled by default, please enable `enable-grpc-gateway` explicitly.

```json
# etcd.json
{
    "enable-grpc-gateway": true,
    "data-dir": "/path/to/data"
}
```

Indeed this distinction was eliminated by etcd in their master branch, but not backport to announced versions, so be care when deploy your etcd cluster.

## How to set up high available Apache APISIX clusters?

The high availability of APISIX can be divided into two parts:

1. The data plane of Apache APISIX is stateless and can be elastically scaled at will. Just add a layer of LB in front.

2. The control plane of Apache APISIX relies on the highly available implementation of `etcd cluster` and does not require any relational database dependency.
