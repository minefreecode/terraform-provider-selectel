# Конфигурации

Файлы конфигураций имеют расширение `md`

Конфигурации имеют вид
```tf
resource "selectel_vpc_project_v2" "webservice" {
name = "webservice"

quotas {
resource_name = "compute_cores"
resource_quotas {
region = "ru-1"
zone = "ru-1a"
value = 4
}
resource_quotas {
region = "ru-2"
zone = "ru-2a"
value = 6
}
}

output "webservice_floating_ip_ru2_1_id" {
  value = "${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.id}"
}

output "webservice_floating_ip_ru2_1_address" {
  value = "${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.floating_ip_address}"
}
```

В конфигурациях имеются переменные ``${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.floating_ip_address}``
которые приходят из модуля ресурсов. Как ресурс опеределишь такая переменная и придёт