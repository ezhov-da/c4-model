# c4-model

```
Context -> Container -> Component -> Code
```

## Context

Показывает программную систему, которую вы создаете, и то, как она вписывается в мир с точки зрения людей, 
которые ее используют, и других программных систем, с которыми она взаимодействует. 

Цветовая кодировка на диаграмме указывает, какие программные системы уже существуют (серые прямоугольники) и какие будут построены (синие).

```plantuml
@startuml C4_Elements
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

Person(personAlias, "Personal Banking Customer", "A customer of the bank, with personal bank accounts")

System(bankingSystem, "Internet Banking System", "Allow customers ti view information")

System_Ext(email, "E-mail System", "The internal Microsoft Exchange e-mail system")
System_Ext(mainframe, "Mainframe Banking System", "Stores all of the core banking information about customers")

Rel(personAlias, bankingSystem, "Uses")

Rel(bankingSystem, mainframe, "Uses")
Rel(bankingSystem, email, "Send e-mail using", "SMTP")

Rel(email, bankingSystem, "Sends e-mail to")
@enduml
```

![](http://www.plantuml.com/plantuml/svg/RL7Dpjem4BpdARQS-2GW5quzGQCSAZKIAd16ZjaGg_w9lEk6ldsT1FE3ECKxEpCxkzaG2y_1Q2ZMfrAZGSkKVLDMwd16Q9ax-fxdNlIhY-8sr87GIsSvybRIcRpJmWxw9V5PgpelrilT01shNxaHnEWZT2e6nPLNNMHcsGuzEJllnizMAq4Bc4sWqF13s3_ANg08nCwWdyNc5kIj0jS0jmXrP2sWZvcWIye10o6b2vPfzfLb-t_4QERrv3XLqUsdPVGM-Jvgweo3j7RzVHW1A_Yhi0Tb6-UDOENim_XKNdcEHYWTbULoU3nP7M9ADKuI6YeblIFNC9HNsGfxGS19G9FWwzapDcpZXS5eKwNtXxHxTXu9XDRfB382SDltEJI8sWL-B3OiyHlrwpzulzEHy4vywpqI9jedLhhD7kqvxHie4iRmrM6Nt6_4L_caeoHa5zcRY0IZ_mC0)


## Container

Масштабируется в программную систему и показывает контейнеры
    - приложения
    - хранилища данных 
    - микросервисы 
    - т. Д.
составляющие эту программную систему. 

**Технологические решения** также являются ключевой частью этой диаграммы. 

Ниже приведен пример схемы контейнера для системы интернет - банкинга. 
Он показывает, что система интернет-банкинга (пунктирная линия) состоит из пяти контейнеров: веб-приложения на стороне сервера, одностраничного приложения на стороне клиента, мобильного приложения, приложения API на стороне сервера и базы данных.

Веб-приложение представляет собой веб-приложение Java/Spring MVC, которое просто обслуживает статический контент (HTML, CSS и JavaScript), включая контент, составляющий одностраничное приложение. Одностраничное приложение-это угловое приложение, которое работает в веб-браузере клиента и предоставляет все функции интернет-банкинга. Кроме того, клиенты могут использовать кроссплатформенное мобильное приложение Xamarin для доступа к подмножеству функций интернет-банкинга. Как одностраничное приложение, так и мобильное приложение используют API JSON/HTTPS, который предоставляет другое приложение Java/Spring MVC, работающее на стороне сервера. 

Приложение API также взаимодействует с существующей банковской системой мэйнфреймов, используя собственный интерфейс XML/HTTPS, чтобы получать информацию о банковских счетах или совершать транзакции. Приложение API также использует существующую систему электронной почты, если ему необходимо отправлять электронную почту клиентам.


```plantuml
@startuml C4_Elements
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(customer, "Personal Banking Customer", "A customer of the bank")

System_Boundary(sb, "Internet Banking System"){
    Container(webApp, "Web Application", "Java and Spring MVC", "Delivers the static content")
    Container(spa, "Single Page Application", "JavaScript and Angular", "Providersall of the internet banking functionality")
    Container(ma, "Mobile App", "Xamarin", "Mobile device")
    ContainerDb(db, "Database", "Relation database Schema", "Stores users")
    Container(api, "Api Application", "Java and Spring MVC", "Providing functionality")
}

System_Ext(email, "E-mail System", "Description")
System_Ext(mn, "Mainframe Banking", "Description")

Rel(webApp, spa, "Delivers")

Rel(customer, webApp, "Uses", "HTTPS")
Rel(customer, spa, "Uses")
Rel(customer, ma, "Uses")

Rel(spa, api, "Uses", "JSON/HTTPS")
Rel(ma, api, "Uses", "JSON/HTTPS")

Rel(api, db, "Reads from and writes to", "JDBC")

Rel(api, email, "Sends e-mail using", "SMTP")
Rel(api, mn, "Uses", "XML/HTTPS")
@enduml
```

![](http://www.plantuml.com/plantuml/svg/ZL9BRzf04BxxLsmvELA3Bpdr532eIGI9LKEJdj2iFS2g-x1srr1KzRztTi4kbAYgJzREz_ZcSUyyMDygoVAxrLIYGkrTya7eIhOrigttZVkKPHRsmutmsvQt3crbj2VSi38gQoJemBzrlfQ2P_dTRH6UblDPfi1vjFIqoea1GgCTDeHDajdesyjoiNfzN3oiPjFFq-T9UfCa1LfdT5grpXk5zYCR75z0iZ7exq9lM7wg3QWuOXsIcJNpMsISK0CIAWzah5PZq-eQx25rdE2Fci9ezBtM4JMu-Pam-lg8wHxvg6c8yOgSqPyK5NXXTF1yWXqmq3Kh6niqvb_py3n1ANQKDQPmdk0LEqs9ybpAkmQ8KH9R8YjWW-zvb9KLZOzE8xrf9SIE2sjseYOVaBBhMNHjyLDRwrPNGGgay8ShTnNCvuOB6Ns3wXiei8Ai-qADEr7Xtzm9JsiUcWKF71m6mXUKCJUhJu-ihBQe8DHARomw5Yx6NUM0HeGLDkA_9jor_bv_l_fzJt_ubBo59FpqDlnqEuuhSd6cmURcdA5qQ9nIhIqexAxZcf9Gh_rjn2NsQ--gVowtlw6bGnUq7XQBeYJS9IpAHCnLINMLJZnYuzXwqQVovJaxLrR_XaHChDSyBWzgnzRMg3ZmlHKU7VCcyYRZ_8BH3RP4JIGyZhTrfr6LyqL1y1wjz5c6j_ciZt1Fz5R9tm00)

## Component

Масштабируется в отдельный контейнер, чтобы показать компоненты внутри него. 
Эти компоненты должны соответствовать реальным абстракциям (например, группировке кода) в вашей кодовой базе.

//TODO

## Code

Наконец, если вы действительно хотите или нуждаетесь в этом, вы можете увеличить масштаб отдельного компонента, чтобы показать, как этот компонент реализован. 

//TODO