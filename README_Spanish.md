<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Apache APISIX

<img src="https://svn.apache.org/repos/asf/comdev/project-logos/originals/apisix.svg" alt="APISIX logo" height="150px" align="right" />

[![Build Status](https://github.com/apache/apisix/workflows/build/badge.svg?branch=master)](https://github.com/apache/apisix/actions)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/apache/apisix/blob/master/LICENSE)

**Apache APISIX** es un portal API en tiempo real, dinámico y de alto rendimiento.

APISIX proporciona variadas opciones de manejo de tráfico tales como balanceo de cargas, upstream dinámico, comprobación tipo despliegue de canarios (canary release), interrupción de circuitos, autenticación, observabilidad y más.

Usted puede usar Apache APISIX para manejar el tráfico tradicional norte-sur,
así como tráfico este-oeste entre servicios.
También puede usarse como [k8s ingress controller (control de ingreso)](https://github.com/apache/apisix-ingress-controller).

La arquitectura técnica de Apache APISIX:

![](doc/images/apisix.png)

## Communidad

- Lista de Correos: Enviar correos a dev-subscribe@apisix.apache.org, luego siga la respuesta para suscribirse a la Lista de Correos.
- QQ Group - 578997126
- [Slack Workspace](http://s.apache.org/slack-invite) - únase a `#apisix` en nuestro Slack para encontrarse con el equipo y formular preguntas
- ![Twitter Follow](https://img.shields.io/twitter/follow/ApacheAPISIX?style=social) - síganos e interactúe con nosotros usando hashtag `#ApacheAPISIX`
- [bilibili video](https://space.bilibili.com/551921247)
- **Good first issues**:
  - [Apache APISIX](https://github.com/apache/apisix/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [Controlador de ingreso Apache APISIX](https://github.com/apache/apisix-ingress-controller/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [Tablero Apache APISIX](https://github.com/apache/apisix-dashboard/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [Carta Helm Apache APISIX](https://github.com/apache/apisix-helm-chart/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [Distribución de Dockers para APISIX](https://github.com/apache/apisix-docker/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [Sitio Web Apache APISIX](https://github.com/apache/apisix-website/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  - [El Plano de Control para APISIX](https://github.com/apache/apisix-control-plane/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)

## Características

Usted puede usar Apache APISIX como un acceso de tráfico para procesar todos los datos del negocio, incluyendo direccionamiento dinámico (dynamic routing), upstream dinámico, certificados dinámicos,
ensayos A/B, ensayos de despliegue de canarios (canary release), despliegue azul-verde (blue-green), límite de tasa, defensa contra ataques maliciosos, métrica, monitoreo de alarmas, visibilidad de servicios, gobernabilidad de servicios, etc.

- **Todas las Plataformas**

  - Nativo de la Nube: Agnóstico de Plataforma, libre de restricciones del vendedor, APISIX puede ejecutarse desde metal desnudo hasta Kubernetes.
  - Entorno de Ejecución: Soporta tanto OpenResty como Tengine.
  - Soporta ARM64: No hay que preocuparse por las restricciones de la infra tecnología.

- **Multi protocolos**

  - [Proxy TCP/UDP](doc/stream-proxy.md): Proxy TCP/UDP dinámico.
  - [Proxy Dubbo](doc/plugins/dubbo-proxy.md): Proxy dinámico HTTP a Dubbo.
  - [Proxy MQTT Dinámico](doc/plugins/mqtt-proxy.md): Soporte de balance de carga MQTT por `client_id`, soporta ambos MQTT [3.1.\*](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html), [5.0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html).
  - [Proxy gRPC](doc/grpc-proxy.md): Tráfico gRPC a través de Proxy.
  - [Transcodificado gRPC](doc/plugins/grpc-transcode.md): Soporta transcodificado de protocolo para que los clientes puedan acceder su gRPC API usando HTTP/JSON.
  - Proxy de Websocket
  - Proxy de Protocolo
  - Proxy Dubbo: Proxy de Dubbo basado en Tengine.
  - Proxy de HTTP(S) hacia adelante
  - [SSL](doc/https.md): Carga dinámica de certificado SSL.

- **Completamente Dinámico**

  - [Las actualizaciones y los plugins más recientes](doc/plugins.md): Actualiza sus configuraciones y plugins sin reinicios!
  - [Reescritura de proxy](doc/plugins/proxy-rewrite.md): Soporta la reescritura de `host`, `uri`, `schema`, `enable_websocket`, `headers` para el request antes de reenviarlo aguas arriba (upstream).
  - [Reescritura de respuesta](doc/plugins/response-rewrite.md): Establece el código de estatus de respuesta personalizado, así como el cuerpo y el encabezado para el cliente.
  - [Sin servidor (serverless)](doc/plugins/serverless.md): Invoque funciones en cada fase en APISIX.
  - Balance dinámico de cargas: Balanceo de cargas Round-robin usando pesos.
  - Balance de cargas basado en Hash: Balanceo de cargas con sesiones de hashing consistentes.
  - [Comprobaciones del sistema](doc/health-check.md): Permite comprobaciones del sistema en el nodo aguas arriba, y automáticamente filtrará nodos problemáticos durante el balanceo de cargas para asegurar la estabilidad del sistema.
  - Interruptor del circuito: Rastreo inteligente de servicios aguas arriba que tengan problemas.
  - [Reflejo de proxy (mirror)](doc/plugins/proxy-mirror.md): Proporciona la capacidad de reflejar (mirror) los request (solicitudes) del cliente.
  - [Bifurcación de tráfico](doc/plugins/traffic-split.md): Permite a los usuarios dirigir de manera creciente porciones del tráfico entre varias corrientes aguas arriba (upstreams).

- **Enrutamiento con control fino (fine grain)**

  - [Soporta correspondencia completa de la ruta y correspondencia del prefijo](doc/router-radixtree.md#how-to-use-libradixtree-in-apisix)
  - [Soporta todas las variables integradas en Nginx como condiciones para el enrutamiento](/doc/router-radixtree.md#how-to-filter-route-by-nginx-builtin-variable), así que pueden usarse `cookie`, `args`, etc. como condiciones de enrutamiento para implementar ensayos de desplegado de canarios, ensayoss A/B, etc.
  - Soporta [varios operadores como condiciones de juicio para el enrutamiento](https://github.com/iresty/lua-resty-radixtree#operator-list), por ejemplo `{"arg_age", ">", 24}`
  - Soporta [función personalizada de correspondencia de ruta](https://github.com/iresty/lua-resty-radixtree/blob/master/t/filter-fun.t#L10)
  - IPv6: Usar IPv6 para hacer corresponder la ruta.
  - Soporta [TTL](doc/admin-api.md#route)
  - [Soporta prioridades](doc/router-radixtree.md#3-match-priority)
  - [Soporta solicitudes Batch Http (por lotes)](doc/plugins/batch-requests.md)

- **Seguridad**

  - Autenticaciones: [key-auth](doc/plugins/key-auth.md), [JWT](doc/plugins/jwt-auth.md), [basic-auth](doc/plugins/basic-auth.md), [wolf-rbac](doc/plugins/wolf-rbac.md)
  - [IP Whitelist/Blacklist](doc/plugins/ip-restriction.md)
  - [Referente Whitelist/Blacklist (listas blancas y negras)](doc/plugins/referer-restriction.md)
  - [IdP](doc/plugins/openid-connect.md): Soporta servicios externos de autenticación, tales como Auth0, okta, etc., los usuarios pueden usar esto para conectarse a OAuth 2.0 y otros métodos de autenticación.
  - [Límite de procesamiento de solicitudes (Limit-req)](doc/plugins/limit-req.md)
  - [Límite de contador (Limit-count)](doc/plugins/limit-count.md)
  - [Límite de concurrencia (Limit-concurrency)](doc/plugins/limit-conn.md)
  - Anti-ReDoS(Negación regular del servicio): políticas integradas para Anti ReDoS sin configuración.
  - [CORS](doc/plugins/cors.md) Activa CORS(Compartir recursos de origen cruzado) para su API.
  - [Bloqueador URI](doc/plugins/uri-blocker.md): Bloquea solicitudes del cliente por URI.
  - [Validador de solicitudes (Request Validator)](doc/plugins/request-validation.md)

- **OPS friendly**

  - OpenTracing: support [Apache Skywalking](doc/plugins/skywalking.md) and [Zipkin](doc/plugins/zipkin.md)
  - works with external service discovery：In addition to the built-in etcd, it also supports `Consul` and `Nacos` [DNS discovery mode](https://github.com/apache/apisix/issues/1731#issuecomment-646392129), and [Eureka](doc/discovery.md)
  - Monitoring And Metrics: [Prometheus](doc/plugins/prometheus.md)
  - Clustering: APISIX nodes are stateless, creates clustering of the configuration center, please refer to [etcd Clustering Guide](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/clustering.md).
  - High availability: support to configure multiple etcd addresses in the same cluster.
  - [Dashboard](https://github.com/apache/apisix-dashboard)
  - Version Control: Supports rollbacks of operations.
  - CLI: start\stop\reload APISIX through the command line.
  - [Stand-alone mode](doc/stand-alone.md): Supports to load route rules from local yaml file, it is more friendly such as under the kubernetes(k8s).
  - [Global Rule](doc/architecture-design.md#global-rule): Allows to run any plugin for all request, eg: limit rate, IP filter etc.
  - High performance: The single-core QPS reaches 18k with an average delay of less than 0.2 milliseconds.
  - [Fault Injection](doc/plugins/fault-injection.md)
  - [REST Admin API](doc/admin-api.md): Using the REST Admin API to control Apache APISIX, which only allows 127.0.0.1 access by default, you can modify the `allow_admin` field in `conf/config.yaml` to specify a list of IPs that are allowed to call the Admin API. Also note that the Admin API uses key auth to verify the identity of the caller. **The `admin_key` field in `conf/config.yaml` needs to be modified before deployment to ensure security**.
  - External Loggers: Export access logs to external log management tools. ([HTTP Logger](doc/plugins/http-logger.md), [TCP Logger](doc/plugins/tcp-logger.md), [Kafka Logger](doc/plugins/kafka-logger.md), [UDP Logger](doc/plugins/udp-logger.md))
  - [Helm charts](https://github.com/apache/apisix-helm-chart)

- **Highly scalable**
  - [Custom plugins](doc/plugin-develop.md): Allows hooking of common phases, such as `rewrite`, `access`, `header filer`, `body filter` and `log`, also allows to hook the `balancer` stage.
  - Custom load balancing algorithms: You can use custom load balancing algorithms during the `balancer` phase.
  - Custom routing: Support users to implement routing algorithms themselves.

## Get Started

### Configure and Installation

APISIX Installed and tested in the following systems:

CentOS 7, Ubuntu 16.04, Ubuntu 18.04, Debian 9, Debian 10, macOS, **ARM64** Ubuntu 18.04

There are several ways to install the Apache Release version of APISIX:

1. Source code compilation (applicable to all systems)
   - Installation runtime dependencies: OpenResty and etcd, and compilation dependencies: luarocks. Refer to [install dependencies documentation](doc/install-dependencies.md)
   - Download the latest source code release package:

     ```shell
     $ mkdir apisix-2.3
     $ wget https://downloads.apache.org/apisix/2.3/apache-apisix-2.3-src.tgz
     $ tar zxvf apache-apisix-2.3-src.tgz -C apisix-2.3
     ```

   - Install the dependencies：

     ```shell
     $ make deps
     ```

   - check version of APISIX:

     ```shell
     $ ./bin/apisix version
     ```

   - start APISIX:

     ```shell
     $ ./bin/apisix start
     ```

2. [Docker image](https://hub.docker.com/r/apache/apisix) （applicable to all systems）

   By default, the latest Apache release package will be pulled:

   ```shell
   $ docker pull apache/apisix
   ```

   The Docker image does not include `etcd`, you can refer to [docker compose example](https://github.com/apache/apisix-docker/tree/master/example) to start a test cluster.

3. RPM package（only for CentOS 7）
   - Installation runtime dependencies: OpenResty, etcd and OpenSSL develop library, refer to [install dependencies documentation](doc/install-dependencies.md#centos-7)
   - install APISIX：

   ```shell
   $ sudo yum install -y https://github.com/apache/apisix/releases/download/2.3/apisix-2.3-0.x86_64.rpm
   ```

   - check version of APISIX:

     ```shell
     $ apisix version
     ```

   - start APISIX:

     ```shell
     $ apisix start
     ```

**Note**: Apache APISIX would not support the v2 protocol of etcd anymore since APISIX v2.0, and the minimum etcd version supported is v3.4.0. Please update etcd when needed. If you need to migrate your data from etcd v2 to v3, please follow [etcd migration guide](https://etcd.io/docs/v3.4.0/op-guide/v2-migration/).

### For Developer

1. For developers, you can use the latest master branch to experience more features

   - build from source code

   ```shell
   $ git clone git@github.com:apache/apisix.git
   $ cd apisix
   $ make deps
   ```

   - Docker image

   ```shell
   $ git clone https://github.com/apache/apisix-docker.git
   $ cd apisix-docker
   $ sudo docker build -f alpine-dev/Dockerfile .
   ```

2. Getting start

   The getting started guide is a great way to learn the basics of APISIX, just follow the steps in [Getting Started](doc/getting-started.md).

   Further, you can follow the documentation to try more [plugins](doc/README.md#plugins).

3. Admin API

   Apache APISIX provides [REST Admin API](doc/admin-api.md) to dynamic control the Apache APISIX cluster.

4. Plugin development

   You can refer to [plugin development guide](doc/plugin-develop.md), and [sample plugin `echo`](doc/plugins/echo.md) documentation and code implementation.

   Please note that Apache APISIX plugins' added, updated, deleted, etc. are hot loaded, without restarting the service.

For more documents, please refer to [Apache APISIX Document Index](doc/README.md)

## Benchmark

Using AWS's 8 core server, APISIX's QPS reach to 140,000 with a latency of only 0.2 ms.

[Benchmark script](benchmark/run.sh), [test method and process](https://gist.github.com/membphis/137db97a4bf64d3653aa42f3e016bd01) has been open source, welcome to try and contribute.

## Apache APISIX vs Kong

#### Both of them have been covered core features of API gateway

| **Features**         | **Apache APISIX** | **KONG** |
| :------------------- | :---------------- | :------- |
| **Dynamic upstream** | Yes               | Yes      |
| **Dynamic router**   | Yes               | Yes      |
| **Health check**     | Yes               | Yes      |
| **Dynamic SSL**      | Yes               | Yes      |
| **L4 and L7 proxy**  | Yes               | Yes      |
| **Opentracing**      | Yes               | Yes      |
| **Custom plugin**    | Yes               | Yes      |
| **REST API**         | Yes               | Yes      |
| **CLI**              | Yes               | Yes      |

#### The advantages of Apache APISIX

| **Features**                                                    | **Apache APISIX**                                 | **Kong**                |
| :-------------------------------------------------------------- | :------------------------------------------------ | :---------------------- |
| Belongs to                                                      | Apache Software Foundation                        | Kong Inc.               |
| Tech Architecture                                               | Nginx + etcd                                      | Nginx + postgres        |
| Communication channels                                          | Mail list, Wechat group, QQ group, GitHub, meetup | GitHub, freenode, forum |
| Single-core CPU, QPS(enable limit-count and prometheus plugins) | 18000                                             | 1700                    |
| Latency                                                         | 0.2 ms                                            | 2 ms                    |
| Dubbo                                                           | Yes                                               | No                      |
| Configuration rollback                                          | Yes                                               | No                      |
| Route with TTL                                                  | Yes                                               | No                      |
| Plug-in hot loading                                             | Yes                                               | No                      |
| Custom LB and route                                             | Yes                                               | No                      |
| REST API <--> gRPC transcoding                                  | Yes                                               | No                      |
| Tengine                                                         | Yes                                               | No                      |
| MQTT                                                            | Yes                                               | No                      |
| Configuration effective time                                    | Event driven, < 1ms                               | polling, 5 seconds      |
| Dashboard                                                       | Yes                                               | No                      |
| IdP                                                             | Yes                                               | No                      |
| Configuration Center HA                                         | Yes                                               | No                      |
| Speed limit for a specified time window                         | Yes                                               | No                      |
| Support any Nginx variable as routing condition                 | Yes                                               | No                      |

Benchmark comparison test [details data](https://gist.github.com/membphis/137db97a4bf64d3653aa42f3e016bd01)

### Contributor Over Time

![contributor-over-time](./doc/images/contributor-over-time.png)

## Videos And Articles

- [Apache APISIX: How to implement plugin orchestration in API Gateway](https://www.youtube.com/watch?v=iEegNXOtEhQ)
- [Improve Apache APISIX observability with Apache Skywalking](https://www.youtube.com/watch?v=DleVJwPs4i4)
- [APISIX technology selection, testing and continuous integration](https://medium.com/@ming_wen/apache-apisixs-technology-selection-testing-and-continuous-integration-313221b02542)
- [Analysis of Excellent Performance of Apache APISIX Microservices Gateway](https://medium.com/@ming_wen/analysis-of-excellent-performance-of-apache-apisix-microservices-gateway-fc77db4090b5)

## User Stories

- [European Factory Platform: API Security Gateway – Using APISIX in the eFactory Platform](https://www.efactory-project.eu/post/api-security-gateway-using-apisix-in-the-efactory-platform)
- [ke.com: How to Build a Gateway Based on Apache APISIX(Chinese)](https://mp.weixin.qq.com/s/yZl9MWPyF1-gOyCp8plflA)
- [360: Apache APISIX Practice in OPS Platform(Chinese)](https://mp.weixin.qq.com/s/zHF_vlMaPOSoiNvqw60tVw)
- [HelloTalk: Exploring Globalization Based on OpenResty and Apache APISIX(Chinese)](https://www.upyun.com/opentalk/447.html)
- [Tencent Cloud: Why choose Apache APISIX to implement the k8s ingress controller?(Chinese)](https://www.upyun.com/opentalk/448.html)
- [aispeech: Why we create a new k8s ingress controller?(Chinese)](https://mp.weixin.qq.com/s/bmm2ibk2V7-XYneLo9XAPQ)

## Who Uses APISIX?

A wide variety of companies and organizations use APISIX for research, production and commercial product, including:

<img src="https://raw.githubusercontent.com/api7/website-of-API7/master/user-wall.jpg" width="900" height="500">

Users are encouraged to add themselves to the [Powered By](doc/powered-by.md) page.

## Landscape

<p align="left">
<img src="https://landscape.cncf.io/images/left-logo.svg" width="150">&nbsp;&nbsp;<img src="https://landscape.cncf.io/images/right-logo.svg" width="200">
<br><br>
APISIX enriches the <a href="https://landscape.cncf.io/category=api-gateway&format=card-mode&grouping=category">
CNCF API Gateway Landscape.</a>
</p>

## Logos

- [Apache APISIX logo(PNG)](logos/apache-apisix.png)
- [Apache APISIX logo source](https://apache.org/logos/#apisix)

## Acknowledgments

Inspired by Kong and Orange.

## License

[Apache 2.0 License](LICENSE)
