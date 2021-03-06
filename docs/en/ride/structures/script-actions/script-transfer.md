# ScriptTransfer

`ScriptTransfer` is a structure that sets the parameters of the token transfer. The token transfer is performed only if the structure is included in the [callable function result](/en/ride/functions/callable-function#invocation-result-2).

### Constructor

``` ride
ScriptTransfer(recipient: Address|Alias, amount: Int, asset: ByteVector|Unit)
```

### Fields

|   #   | Name | Data type | Description |
| :--- | :--- | :--- | :--- |
| 1 | recipient | [Address](/en/ride/structures/common-structures/address)&#124;[Alias](/en/ride/structures/common-structures/alias) | [Address](/en/blockchain/account/address) or the [alias](/en/blockchain/account/alias) of a recipient of tokens |
| 2 | amount | [Int](/en/ride/data-types/int) | Number of tokens |
| 3 | asset | [ByteVector](/en/ride/data-types/byte-vector)&#124;[Unit](/en/ride/data-types/unit) | ID of a token |
