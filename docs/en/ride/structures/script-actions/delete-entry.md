# DeleteEntry

> :warning: The structure is added in Standard library **version 4** which becomes available since node version 1.2.0, after activation of feature #15 “Ride V4, VRF, Protobuf, Failed transactions”.

`DeleteEntry` is a structure that sets the paramaters of deletion of entry from the [account data storage](/en/blockchain/account/account-data-storage). Deleting an entry is performed only if the structure is included in the [callable function result](/en/ride/functions/callable-function#invocation-result-2).

## Constructor

`DeleteEntry(key: String)`

## Fields

|   #   | Name | Data type | Description |
| :--- | :--- | :--- | :--- |
| 1 | key | [String](/en/ride/data-types/string) | Entry key. The maximum size is 400 bytes |

## Example

```ride
{-# STDLIB_VERSION 4 #-}
{-# SCRIPT_TYPE ACCOUNT #-}
    
@Callable(inv)
func default() = {
  [ DeleteEntry(inv.caller.toString()) ]
}
```
