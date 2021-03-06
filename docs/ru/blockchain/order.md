# Биржевая заявка

**Биржевая заявка** или **ордер** — инструкция [владельца аккаунта](/ru/blockchain/account/) [матчеру](https://docs.waves.exchange/ru/waves-matcher/) на покупку или продажу [токена](/ru/blockchain/token/) на [Waves Exchange](https://waves.exchange/).

## Ассетная пара

Каждая биржевая заявка содержит пару amount-ассет/ price-ассет, называемую также ассетной парой.

### Пример

```js
"assetPair": {
    "amountAsset": "3QvxP6YFBKpWJSMAfYtL8Niv8KmmKsnpb9uQwQpg8QN2",
    "priceAsset": "null"
}
```

### Поля ассетной пары

| Имя поля | Описание |
|---|---|
| amountAsset  | ID первого ассета, который отправитель ордера хочет купить или продать |
| priceAsset | ID второго ассета, в котором выражена цена ордера. Null означает, что ассетом является WAVES |

## Amount и price биржевой заявки

В пользовательском интерфейсе amount и price обычно представлены в виде значений с дробной частью (например, 0,74585728 WAVES), т.е. в денормализованном виде. Денормализованный вид нагляден для человека, но [не удобен для вычислений](https://en.wikipedia.org/wiki/Round-off_error). Чтобы решить проблему точности вычислений, выполняется нормализация, т.е. представление amount и price в ордере в виде целого числа. Так, 0,74585728 WAVES — это 0,74585728×10<sup>8</sup> или 74585728 [WAVELET](/ru/blockchain/token/wavelet). В рассмотренном случае показатель степени равен 8, потому что WAVES имеет 8 разрядов (decimals) после запятой. Другие ассеты могут иметь другое значение decimals. Например, у TDX количество decimals равно 2.

### Amount

Рассмотрим покупку 2,13 TDX по цене 0,35016774 WAVES за один TDX. Здесь ассетная пара — TDX / WAVES. Аmount в ордере — это количество продаваемого или покупаемого в условных "копейках". Это значение в рассматриваемом случае равно 213, поскольку 2,13 TDX = 2,13 × 10<sup>2</sup> = 213 "копеек" TDX.

Итак, чтобы привести amount к нормализованному виду, он умножается на 10<sup>amountAssetDecimals</sup>.

### Price

Price представляет собой стоимость 1 единицы amount-ассета, выраженную в price-ассете.

В рассмотренном выше примере с TDX / WAVES это цена в WAVES за 1 TDX.

Чтобы привести price к нормализованному виду, он умножается

* в ордерах версий 1, 2, 3: на 10<sup>(8 + priceAssetDecimals - amountAssetDecimals)</sup>,
* в ордерах версии 4: на 10<sup>8</sup>.

Показатель степени, равный 8, выбран потому, что в блокчейне Waves не может быть ассета с бо́льшим количеством decimals.

Не вдаваясь в подробности, отметим, что предложенный в ордерах четвертой версии способ нормализации price обеспечивает бо́льшую гибкость при работе с ордерными парами, имеющими разное количество decimals.

> Алгоритм работы матчера имеет ограничение в отношении price:  последние цифры нормализованного price в количестве (price_decimals - amount_decimals) должны быть нулями. Если это не так, то матчер отклоняет ордер при постановке.

### Расчет количества price-ассета

Количество price-ассета в нормализованном виде, которое отправитель

* отдаст в случае BUY-ордера
* получит в случае SELL-ордера

вычисляется по следующей формуле

* в ордерах версий 1, 2, 3: amount × price × 10<sup>-8</sup>,
* ордерах версии 4: amount × price × 10<sup>(priceAssetDecimals - amountAssetDecimals - 8)</sup>.

Если в результате вычисления получено значение с дробной частью, то она отбрасывается.

Обозначения в приведенной формуле:

* amount — amount в нормализованном виде,
* price — price в нормализованном виде,
* priceAssetDecimals — количество знаков после запятой у price-ассета,
* amountAssetDecimals — количество знаков после запятой у amount-ассета.

### Пример расчета для ордера v1/v2/v3

Рассмотрим покупку 2,13 TDX по цене 0,35016774 WAVES за один TDX.

Amount-ассетом является TDX, значение amountAssetDecimals равно 2.

Price-ассетом является WAVES, значение priceAssetDecimals равно 8.

Значение amount в нормализованном виде равно 2,13 × 10<sup>amountAssetDecimals</sup> = 213.

Значение price в нормализованном виде равно 0,35016774 × 10<sup>(8 + 8 - 2)</sup> = 35016774000000.

Количество price-ассета в нормализованном виде будет рассчитано следующим образом:  213 × 35016774000000 × 10<sup>-8</sup> = 74585728,62. Дробная часть отбрасывается.

В результате за 2,13 TDX будет выручено 0,74585728 WAVES.

### Пример расчета для ордера v4

Рассмотрим покупку 2,13 TDX по цене 0,35016774 WAVES за один TDX.

Amount-ассетом является TDX, значение amountAssetDecimals равно 2.

Price-ассетом является WAVES, значение priceAssetDecimals равно 8.

Значение amount в нормализованном виде равно 2,13 × 10<sup>amountAssetDecimals</sup> = 213.

Значение price в нормализованном виде равно 0,35016774 × 10<sup>8</sup> = 35016774.

Количество price-ассета в нормализованном виде будет рассчитано следующим образом:  213 × 35016774 × 10<sup>(8 - 2 - 8)</sup> = 74585728,62. Дробная часть отбрасывается.

В результате за 2,13 TDX будет выручено 0,74585728 WAVES.

## Отмена заявки <a id="cancel"></a>

Отправитель заявки может отменить заявку до того, как она будет исполнена.

Не исполненные заявки автоматически отменяются в дату и время, указанную отправителем заявки.

## Дата истечения заявки

**Дата истечения заявки** — дата и время автоматической аннуляции неисполненной заявки.

Указывается в миллисекундах с начала [эпохи Unix](https://ru.wikipedia.org/wiki/Unix-время).

Дата истечения заявки не может быть меньше текущей даты и времени на матчере + 1 минута и больше текущей даты и времени на матчере + 30 дней.

## Временная метка заявки

**Временна́я метка заявки** — время, когда [матчер](https://docs.waves.exchange/ru/waves-matcher/) добавил [ордер](/ru/blockchain/order) в [биржевой стакан](https://ru.wikipedia.org/wiki/Биржевой_стакан) на Waves Exchange.

Время указывается в миллисекундах, которые прошли с начала [эпохи Unix](https://ru.wikipedia.org/wiki/Unix-время).

## Бинарный формат ордера

Смотрите страницу [Бинарный формат ордера](/ru/blockchain/binary-format/order-binary-format).
