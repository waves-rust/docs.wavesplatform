# Бинарный формат транзакции спонсирования

> Узнать больше о [спонсировании](/ru/blockchain/waves-protocol/sponsored-fee).

## Версия 2

Бинарный формат версии 2 соответствует protobuf-схеме [transaction.proto](https://github.com/wavesplatform/protobuf-schemas/blob/master/proto/waves/transaction.proto). Описание полей, общих для всех типов транзакций, представлено в разделе [Бинарный формат транзакции](/ru/blockchain/binary-format/transaction-binary-format/).

Версия 2 добавлена в версии ноды 1.2.0 и включается с активацией фичи № 15 “Ride V4, VRF, Protobuf, Failed transactions”.

```
message SponsorFeeTransactionData {
    Amount min_fee = 1;
};

message Amount {
    bytes asset_id = 1;
    int64 amount = 2;
};
```

| Поле | Размер | Описание |
| :--- | :--- | :--- |
| min_fee.asset_id | 32 байта | ID ассета |
| min_fee.amount | 8 байт | Количество спонсорского ассета, эквивалентное 0,001 WAVES (100&nbsp;000 WAVELET), в минимальных единицах («копейках») ассета. См. раздел [Спонсирование комиссии](/ru/blockchain/waves-protocol/sponsored-fee) |

## Версия 1

| № | Поле | Название JSON-поля | Тип поля | Размер поля в байтах | Комментарий |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Флаг версии |  | [Byte](/ru/blockchain/blockchain/blockchain-data-types) | 1 | Указывает, что версия транзакции является второй или выше.<br>Значение должно быть равно 0 |
| 2 | [ID типа транзакции](/ru/blockchain/transaction-type/) | type | [Byte](/ru/blockchain/blockchain/blockchain-data-types) | 1 | Значение должно быть равно 14 |
| 3 | Версия транзакции | version | [Byte](/ru/blockchain/blockchain/blockchain-data-types) | 1 | Значение должно быть равно 1 |
| **4** | ID типа транзакции | type | [Byte](/ru/blockchain/blockchain/blockchain-data-types) | 1 | Дублирует поле 2 |
| **5** | Версия транзакции | version | [Byte](/ru/blockchain/blockchain/blockchain-data-types) | 1 | Дублирует поле 3 |
| **6** | Открытый ключ аккаунта отправителя транзакции | chainId | Array[[Byte](/ru/blockchain/blockchain/blockchain-data-types)] | 32 |  |
| **7** | [ID](/ru/blockchain/token/token-id) спонсируемого токена | assetId | Array[[Byte](/ru/blockchain/blockchain/blockchain-data-types)] | 32 |  |
| **8** | Минимальная комиссия | minSponsoredAssetFee | [Long](/ru/blockchain/blockchain/blockchain-data-types) | 8 | Количество спонсируемого токена, которое будет равно 0,001 WAVES (100&nbsp;000 WAVELET) при оплате этим токеном |
| **9** | [Комиссия за транзакцию](/ru/blockchain/transaction/transaction-fee) | fee | [Long](/ru/blockchain/blockchain/blockchain-data-types) | 8 |  |
| **10** | Временная метка транзакции | timestamp | [Long](/ru/blockchain/blockchain/blockchain-data-types) | 8 |  |
| 11 | [Подтверждения транзакции](/ru/blockchain/transaction/transaction-proof) | proofs | См. раздел [Бинарный формат подтверждений](/ru/blockchain/binary-format/transaction-proof-binary-format) | `S` | Если массив пустой, то `S` = 3. <br>Если массив не пустой, то `S` = 3 + 2 × `N` + 64 × `N`, где `N` — количество подтверждений в массиве.<br>Максимальное количество подтверждений в массиве — 8. Размер каждого подтверждения — 64 байта |

> Поля, номера которых выделены жирным шрифтом, составляют байты тела транзакции.

## JSON-представление транзакции

Смотрите [пример](https://nodes.wavesnodes.com/transactions/info/7EL2XEGP1By427BeLcHPYeVnBzGsXen4egMAwQpWGBVR) в Node API.
