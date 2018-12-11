![Logo](admin/onvif_logo.png)
# ioBroker.onvif
=================

## Настройка
1. Открыть Настройки драйвера
2. Нажать кнопку сканирования (сверху справа)
3. Ввести необходимые настройки или оставить по умолчанию
    startRange - начальный ip адрес диапазона сканирования
    End Range - конечный ip адрес диапазона сканирования
    Port list - через запятую порты сервиса onvif (по умолчанию: 80, 7575, 8000, 8080, 8081)
    User name - по умолчанию admin
    Passord - по умолчанию admin
4. Нажать START SCAN

Если все сделано правильно, в основном окне настроек появятся найденые камеры и через несколько секунд должны будут подтянуться снапшоты

## События
Драйвер автоматически подписывается на события к настроенным камерам.
События, которые генерирует камера, появятся в объектах вида:

```
onvif.0.192_168_1_4_80.message.tns1:RuleEngine/FieldDetector/ObjectsInside
onvif.0.192_168_1_4_80.message.tns1:VideoSource/MotionAlarm.State
```

## Запрос снапшота
Для этого используется команда:
```
sendTo('onvif.0', command, message, callback);
```
Пример скрипта для запроса снапшота и отправка в телеграм:
```
const fs = require('fs');

function getSnapshot(caption){
    sendTo('onvif.0', 'saveFileSnapshot', {"id":"onvif.0.192_168_1_4_80", "file":"/opt/cameras/snapshot.jpg"}, (data) => {
        console.log('image принят: ' + data);
        if (data === "OK")
            sendTo('telegram.0', {text: '/opt/cameras/snapshot.jpg', caption: caption});
    });
}
```
*caption* - заголовок для картинки в телеграме

Вызывать можно как по событию, так и по кнопке/рассписанию

## Changelog
### 0.0.2 (2018-11-20)
* (haba1234) add events and snapshot

### 0.0.1 (2018-02-20)
* (Kirov Ilya) intial commit

## License

The MIT License (MIT)

Copyright (c) 2018 Kirov Ilya <kirovilya@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
