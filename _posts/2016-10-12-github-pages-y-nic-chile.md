---
layout: post
title: Github pages y NIC Chile
Introduction: Como configurar NIC Chile con Github pages
comments: true
category: spanish
---
Para este post, quiero explicar como configurar una página o sitio web creado en github pages con un dominio ".cl" adquirido en NIC Chile.


Tras crear en [NIC Chile](http://nic.cl) el dominio para mi empresa, [pacificlabs.cl](http://pacificlabs.cl), comencé a hacer la página del sitio, para lo que decidí usar [GitHub pages](https://pages.github.com/), por su simpleza, versatilidad (temas) y gratuidad.

Una vez terminada la página web, y disponible en el sitio de github [http://slarrain.github.io/pacificlabs](http://slarrain.github.io/pacificlabs), quedaba hacer el link entre Github pages y el sitio de NIC Chile. Y sin embargo, no parecía ni tan fácil ni tan simple, ni pude encontrar un manual que lo explicara paso a paso. Por lo que decidí hacer un tutorial para quien lo pueda necesitar en el futuro.

Lo primero, es que este manual asume 2 cosas:

1. Ya tienen un dominio registrado en NIC Chile, como [pacificlabs.cl](http://pacificlabs.cl).
2. Ya tienen una página web funcionando en Github pages.

Lo primero es bastante simple y no requiere mucha ayuda. Lo segundo es un poco más complicado y tendrán que experimentar con Jekyll y git, pero hay cientos de tutoriales en internet que explican como hacerlo. El objetivo de este tutorial es explicar cómo hacer para que, si yo ingreso el dominio .cl, se me muestre la página de github pages.

### 1. DNS

Lo primero que hay que hacer es usar un servidor DNS. Si no saben muy bien qué significa eso, no hay problema, sólo necesitamos usarlo y aquí explicaré cómo. Hay muchos servidores DNS gratuitos y pueden usar el que Uds. quieran. Aquí explicaré paso a paso con el servidor que usé yo, [CloudDNS](https://www.cloudns.net/). Es gratis y me ha funcionando sin problemas, por lo que lo recomiendo.

Lo primero es hacerse una cuenta en CloudDNS. Una vez que ingresamos a nuestro Dashboard, vamos a "DNS zones" -> "Add new":

![Add new DNS zone]({{ site.baseurl }}/imgs/new_dns_zone.png)


Luego seleccionamos "Master Zone":

![Add new DNS zone]({{ site.baseurl }}/imgs/master_zone.png)


Y luego en "Domain name" escribimos el nombre de nuestro dominio. En mi caso, es [pacificlabs.cl](http://pacificlabs.cl)

![New Domain zone]({{ site.baseurl }}/imgs/new_domain_zone.png)

### 2. A Records

Después de esto, debemos crear 2 "A records" con las direcciones IP públicas de Github pages.
En el mismo Dashboard de CloudDNS, vamos a la pestaña "A" y creamos un nuevo record. Debemos hacer esto 2 veces, y poner 2 IPs distintas cada vez. Las IPs son:
- 192.30.252.154
- 192.30.252.153

El campo de "Host" lo dejamos vacío.

![A record]({{ site.baseurl }}/imgs/a_record.png)

### 3. CNAME record

A continuación, en el mismo Dashboard de CloudDNS, debemos crear un "CNAME record", pero esta vez, en "Host" debemos ingresar __"www"__. Esto es, porque nos interesa que tanto pacificlabs.cl como www.pacificlabs.cl redireccionen al mismo sitio y ambos funcionen.

En el campo "Points to", no debemos poner una IP, sino nuestro sitio en github, sin el nombre del proyecto (el /pacificlabs/), y con un punto __"."__ al final. En mi caso, como mi nombre de usuario es "slarrain", debia ingresar __"slarrain.github.io."__

![CNAME record]({{ site.baseurl }}/imgs/cname_record.png)

### 4. CNAME file

Con esto terminamos el uso de CloudDNS y estamos casi listos. Ahora solo debemos crear un archivo llamado "CNAME" (sin extensión) en el directorio base de nuestro proyecto, y dentro de él debemos agregar el dominio de NIC Chile, y nada más:

`pacificlabs.cl`

Luego git add, commit & push.

### 5. NIC Chile

Sólo nos queda avisarle a NIC Chile cual será nuestro servidor DNS. Ingresamos con nuestra cuenta y vamos a "4. Servidores de Nombre (DNS)". Ahí debemos ingresar las 4 direciones DNS que CloudDNS nos daba al agregar la "DNS zone". Simple y rápido:

![NIC dns]({{ site.baseurl }}/imgs/nic_dns.png)

### 6. Esperar

__¡Listo!__ Ahora solo nos queda esperar a que se propague el DNS. Dicen que puede tardar horas, pero a mi me funcionaba en menos de 5 minutos.

Mucha suerte y espero que les haya servido.

Cualquier duda o comentario, son muy bienvenidos.

[//]: # (Buena parte de la información fue sacada de -como no- stackoverflow: http://stackoverflow.com/questions/9082499/custom-domain-for-github-project-pages)
