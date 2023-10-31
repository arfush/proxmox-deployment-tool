# Proxmox Deployment Tool

Тулка для массовой работы с кучей клонов Proxmox/Debian.

Есть три действия:
	* Deploy - деплой машин
	* Destroy - удалить машины
	* Snapshot - создать снапшот
	* Rollback - откатить к снапшоту

Для использования нужно создать ВМ и привязать к ней этот hook. Зальём его в local

```shell
qm set 100 --hookscript local:snippets/hook
```

Потом идем в Proxmox, меняем Notes нашей ВМ.
Вот примеры:

```yaml
action: deploy
template:
  id: 10002 # Айди нашего шаблона
  source: host04 # Нода где находится шаблон
count: 20
id: # С этой конфой у нас будут машины типа 7001, 7011
  prefix: 70 # Префикс наших ВМ 
  offset: 1 # Оффест. Типа, если у вас уже есть машина 7001, то можете поставить тут 2. Тогда ваши клоны будут начинаться с 7002
ip:
  offset: 51 # Оффест IP-адреса. Сеть 172.16.100.0/24. В коде поменяете если надо будет
snapshot: DEFAULT # Снапшот который будет создаваться после настройки машины
```

```yaml
action: destroy
id:
  prefix: 70
  offset: 2 # Можете удалить, если вам нужна машина 7001, все начиная с 7002. ИМБА ЖЕ
```

```yaml
action: snapshot
id:
	prefix: 70
	offset: 1
snapshot: NEW_SNAP # Название нового снапшота
```

```yaml
action: rollback
id:
	prefix: 70
	offset: 1
snapshot: DEFAULT # К какому снапу откат сделать
```
