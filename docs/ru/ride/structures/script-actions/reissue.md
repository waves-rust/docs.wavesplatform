# Reissue

> :warning: Структура `Reissue` добавлена в [Стандартной библиотеке](/ru/ride/script/standard-library) **версии 4**, которая доступна начиная с версии ноды 1.2.0 после активации фичи №&nbsp;15 “Ride V4, VRF, Protobuf, Failed transactions”.

`Reissue` — cтруктура, задающая параметры довыпуска токена. Довыпуск выполняется, только если структура включена в [результирующее выражение](/ru/ride/functions/callable-function#резуnьтат-выпоnнения-2) вызываемой функции.

## Конструктор

```ride
Reissue(assetId: ByteVector, quantity: Int, isReissuable: Boolean)
```

## Поля

| # | Название | Тип данных | Описание |
| :--- | :--- | :--- | :--- |
| 1 | assetId | [ByteVector](/ru/ride/data-types/byte-vector) | [ID токена](/ru/blockchain/token/token-id) для довыпуска |
| 2 | quantity | [Int](/ru/ride/data-types/int) | Количество токена |
| 3 | isReissuable | [Boolean](/ru/ride/data-types/boolean) | Флаг возможности довыпуска |
