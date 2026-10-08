# Project_template

Это шаблон для решения проектной работы. Структура этого файла повторяет структуру заданий. Заполняйте его по мере работы над решением.

# Задание 1. Анализ и планирование

<aside>

Чтобы составить документ с описанием текущей архитектуры приложения, можно часть информации взять из описания компании и условия задания. Это нормально.

</aside

### 1. Описание функциональности монолитного приложения

**Управление отоплением:**
- Пользователи могут удалённо включать/выключать отопление в своих домах
- Система поддерживает управление отопление запросом к датчику

**Мониторинг температуры:**
- Пользователи могут проверять температуру
- Система поддерживает отображение температуры

### 2. Анализ архитектуры монолитного приложения
Монолитное приложение, язык Go, СУБД PostgreSQL. Обращение из app-уровня в db-уровень синхронное

### 3. Определение доменов и границы контекстов
- Монолитная система (управляет взаимодействием с датчиками - подключение, проверка температуры, включение/выключение отопления)
- Пользователь (посылает запросы на регулирование температуры и отображение текущей температуры)
- Датчик (отвечает на запросы сервера, выставляет по команде сервера температуру)


### **4. Проблемы монолитного решения**
- Ручное подключение системы отопления к системе, необходимость выезда мастера - оплата его работы, потеря времени и лояльности клиентов
- Синхронное однопоточное взаимодействие системы с СУБД, одна поломка заденет все последующие
- Получение данных о температуры через посредника - сетевые издержки, отсутствие соединения рушит смысл решения
- Сложность в масштабировании

### 5. Визуализация контекста системы - диаграмма С4

[![C4_context](//www.plantuml.com/plantuml/png/TL7DojD05DtFKunTLT1cqvMhGhTMQejhIPF13YGpONA3k4ljmaLT2eA8F_e2wKzfciRs5UwyaNScYiZ7HsbcxfbppZrtPfH3QA184mOI4i7n1nxnXXUxmJF70rwn7N3yFdJx6YlSsvA-BVqgZWqT_x9lIT7O5QqLOw0p3felD81EUIoDY41gnTW3gQAaY4LX4hu4oF8dGM32ruDN4fR5eiY5YRG2eM0GwJIOoIWqiNucHIQIO3nyF4r21IycJuCqp44OWbwETPffvHIPoy-cmkHQzPtL8zx3hnJkOj_ZYTOTLZucrJrtzWalM5DDuNiF6Pk_8S-qFmcbwhQEyzaDHtzY4iiXF_7cfzyHHbhdmAau4LHZIzG3mMT_4pjlttAxueHIS_kEaCPVNKqjkpRF2r68_boeNFl87_unVSEFdd-q9VVfMxovmfFMjhGRbtqcaZyZ_n__6mdsTktZbzqLT97lzV9MFimTio-uzPDlyKhgvk_RjzxpGDAHK34zmcy0"test")](//www.plantuml.com/plantuml/png/TL7DojD05DtFKunTLT1cqvMhGhTMQejhIPF13YGpONA3k4ljmaLT2eA8F_e2wKzfciRs5UwyaNScYiZ7HsbcxfbppZrtPfH3QA184mOI4i7n1nxnXXUxmJF70rwn7N3yFdJx6YlSsvA-BVqgZWqT_x9lIT7O5QqLOw0p3felD81EUIoDY41gnTW3gQAaY4LX4hu4oF8dGM32ruDN4fR5eiY5YRG2eM0GwJIOoIWqiNucHIQIO3nyF4r21IycJuCqp44OWbwETPffvHIPoy-cmkHQzPtL8zx3hnJkOj_ZYTOTLZucrJrtzWalM5DDuNiF6Pk_8S-qFmcbwhQEyzaDHtzY4iiXF_7cfzyHHbhdmAau4LHZIzG3mMT_4pjlttAxueHIS_kEaCPVNKqjkpRF2r68_boeNFl87_unVSEFdd-q9VVfMxovmfFMjhGRbtqcaZyZ_n__6mdsTktZbzqLT97lzV9MFimTio-uzPDlyKhgvk_RjzxpGDAHK34zmcy0)

# Задание 2. Проектирование микросервисной архитектуры

В этом задании вам нужно предоставить только диаграммы в модели C4. 
Мы не просим вас отдельно описывать получившиеся микросервисы и то, 
как вы определили взаимодействия между компонентами To-Be системы. 
 Если вы правильно подготовите диаграммы C4, они и так это покажут.

**Диаграмма контейнеров (Containers)**

[![C4_container](//www.plantuml.com/plantuml/png/hLNFRnj55BxlNp7uv4Xfzj2UEBKuY0fIv7O37AknFNKMTdTMwzcM224H9mYLX5PKFI54AGY9e-F6PECactzXvZ_Yc-TrThsn9GuSijcTzzxtlH_VFDyTA9weD0mSvQj0CFs7dkWhVMuEzHdJ9phN5njClxt3pBxv3bzZc2_D4TDJdUDbYCypVQadCwFZA1ap9Lb7AYL3rlCJqLM-Z1pdFJ_g1cbFf4d0TfIA1--xRiAVRVHzjPDsqe58kZ9I8b8RNHcwZ_mkJthoKr_48d1RTswr0nwfpnvikI4VA97Ww75BXh6CR4HiytVQ8Xd8g6cXrbb3lz4voZZIfvJrY5B5TyFQVj7dEgDAXeqrnzi5PDZqGdJYlaIyTNchCxD7GHTwXEhr5KeS0UdSni7uqXvU0E9GNnITidyM7vXE2zLmFnB9umza6lLuilj_4QonRnp6hl6RNuZElJYsaAyGUNoBW3B0vFeVWaAbDglshjgHqJhR_997VTce16GqISpG7DXvcZqKK6Lu83j-80gINu5xh3FgxINU9smLuITI1Ju8lMV-4zKeOqUGpFb_OCM_sMXiw6zcZ-Z6z8GsZyrFUafq9nRxO_wK4yUVPIFDIy0F6IBBLYEM-aemxtG-x-oendulzZ_YYYRrDv1Uw5DcaoVeOvq3RQep6pm24cPalb_8uerj2RK2q9G5l2a9wuyeb3s4zLHdUDidZn9CJnQpsUmqDxdY7Lv81qN-h5_JKEL0zHFXFTmYshyeFz02ZlgGcmCQJRO-7zCONJajYT0CcnRTtpFxPY_BEfjT5-hOn5g1-ABMeS_zdf2UIWGF_QXFSWBy2M9oL4ehOMoVfVPMp0UlHHnwEY96okP63xpRrHgfBFDSJnQci1JD4dktwnsgkD9q3cYi-IBjmrIaeadFcGn_i828EJ57jioSLF6yqFqPwPu_-OetoFjQCDW_wFQuq48z3dUTD344UaQhTOmilgrkaG-_LarSXLWaNAKOk8W6alRSwpAE5Hg_iYdICPQsEQHKVeVbdTcddj6kBdeuMHMqLcVlSpMdNMG5LcuniKoBnOlc_6X1NLBQrEAHsYTLtxKQ_bEhfnOFKI48NYtHnAXOMbDx8K5Ux9ePonLTyYCN0Z-iUzpW2lp-ZbfQZDaZrGvNao7t-c4r2drx98BYP-XwDzDF7cobQRvKwzPvIBd-QWvbu6gVgc6ktUrMsztUyWgFuhHSSp0l2QvTyZfAtKmtzFxsTjkr3sy9O6MFbL1BiIfwAAehuAvpBXMvmg5-OLRvaHpg7dT5r4l3u5y0"test")](//www.plantuml.com/plantuml/png/hLNFRnj55BxlNp7uv4Xfzj2UEBKuY0fIv7O37AknFNKMTdTMwzcM224H9mYLX5PKFI54AGY9e-F6PECactzXvZ_Yc-TrThsn9GuSijcTzzxtlH_VFDyTA9weD0mSvQj0CFs7dkWhVMuEzHdJ9phN5njClxt3pBxv3bzZc2_D4TDJdUDbYCypVQadCwFZA1ap9Lb7AYL3rlCJqLM-Z1pdFJ_g1cbFf4d0TfIA1--xRiAVRVHzjPDsqe58kZ9I8b8RNHcwZ_mkJthoKr_48d1RTswr0nwfpnvikI4VA97Ww75BXh6CR4HiytVQ8Xd8g6cXrbb3lz4voZZIfvJrY5B5TyFQVj7dEgDAXeqrnzi5PDZqGdJYlaIyTNchCxD7GHTwXEhr5KeS0UdSni7uqXvU0E9GNnITidyM7vXE2zLmFnB9umza6lLuilj_4QonRnp6hl6RNuZElJYsaAyGUNoBW3B0vFeVWaAbDglshjgHqJhR_997VTce16GqISpG7DXvcZqKK6Lu83j-80gINu5xh3FgxINU9smLuITI1Ju8lMV-4zKeOqUGpFb_OCM_sMXiw6zcZ-Z6z8GsZyrFUafq9nRxO_wK4yUVPIFDIy0F6IBBLYEM-aemxtG-x-oendulzZ_YYYRrDv1Uw5DcaoVeOvq3RQep6pm24cPalb_8uerj2RK2q9G5l2a9wuyeb3s4zLHdUDidZn9CJnQpsUmqDxdY7Lv81qN-h5_JKEL0zHFXFTmYshyeFz02ZlgGcmCQJRO-7zCONJajYT0CcnRTtpFxPY_BEfjT5-hOn5g1-ABMeS_zdf2UIWGF_QXFSWBy2M9oL4ehOMoVfVPMp0UlHHnwEY96okP63xpRrHgfBFDSJnQci1JD4dktwnsgkD9q3cYi-IBjmrIaeadFcGn_i828EJ57jioSLF6yqFqPwPu_-OetoFjQCDW_wFQuq48z3dUTD344UaQhTOmilgrkaG-_LarSXLWaNAKOk8W6alRSwpAE5Hg_iYdICPQsEQHKVeVbdTcddj6kBdeuMHMqLcVlSpMdNMG5LcuniKoBnOlc_6X1NLBQrEAHsYTLtxKQ_bEhfnOFKI48NYtHnAXOMbDx8K5Ux9ePonLTyYCN0Z-iUzpW2lp-ZbfQZDaZrGvNao7t-c4r2drx98BYP-XwDzDF7cobQRvKwzPvIBd-QWvbu6gVgc6ktUrMsztUyWgFuhHSSp0l2QvTyZfAtKmtzFxsTjkr3sy9O6MFbL1BiIfwAAehuAvpBXMvmg5-OLRvaHpg7dT5r4l3u5y0)

**Диаграмма компонентов (Components)**

Добавьте диаграмму для каждого из выделенных микросервисов.

**Диаграмма кода (Code)**

Добавьте одну диаграмму или несколько.

# Задание 3. Разработка ER-диаграммы

Добавьте сюда ER-диаграмму. Она должна отражать ключевые сущности системы, их атрибуты и тип связей между ними.

# Задание 4. Создание и документирование API

### 1. Тип API

Укажите, какой тип API вы будете использовать для взаимодействия микросервисов. Объясните своё решение.

### 2. Документация API

Здесь приложите ссылки на документацию API для микросервисов, которые вы спроектировали в первой части проектной работы. Для документирования используйте Swagger/OpenAPI или AsyncAPI.

# Задание 5. Работа с docker и docker-compose

Перейдите в apps.

Там находится приложение-монолит для работы с датчиками температуры. В README.md описано как запустить решение.

Вам нужно:

1) сделать простое приложение temperature-api на любом удобном для вас языке программирования, которое при запросе /temperature?location= будет отдавать рандомное значение температуры.

Locations - название комнаты, sensorId - идентификатор названия комнаты

```
	// If no location is provided, use a default based on sensor ID
	if location == "" {
		switch sensorID {
		case "1":
			location = "Living Room"
		case "2":
			location = "Bedroom"
		case "3":
			location = "Kitchen"
		default:
			location = "Unknown"
		}
	}

	// If no sensor ID is provided, generate one based on location
	if sensorID == "" {
		switch location {
		case "Living Room":
			sensorID = "1"
		case "Bedroom":
			sensorID = "2"
		case "Kitchen":
			sensorID = "3"
		default:
			sensorID = "0"
		}
	}
```

2) Приложение следует упаковать в Docker и добавить в docker-compose. Порт по умолчанию должен быть 8081

3) Кроме того для smart_home приложения требуется база данных - добавьте в docker-compose файл настройки для запуска postgres с указанием скрипта инициализации ./smart_home/init.sql

Для проверки можно использовать Postman коллекцию smarthome-api.postman_collection.json и вызвать:

- Create Sensor
- Get All Sensors

Должно при каждом вызове отображаться разное значение температуры

Ревьюер будет проверять точно так же.


