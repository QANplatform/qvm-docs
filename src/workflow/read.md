# Reading contract storage

It is easy to read any database field of any contract.

## Read a single field

To read a single field execute the following command:

```sh
docker run --rm qanplatform/qvmctl query $CONTRACT_ADDR $DB_SLOT
```

Make sure you replace ```$CONTRACT_ADDR``` and ```$DB_SLOT``` with the right values.

## Read multiple fields

To read multiple fields execute the following command:

```sh
docker run --rm qanplatform/qvmctl query $CONTRACT_ADDR $DB_SLOT1,$DB_SLOT2,$DB_SLOTN
```

Make sure you replace ```$CONTRACT_ADDR``` and ```$DB_SLOT{N}``` variables with the right values. Also pay attention that DB_SLOT variable names are separated by a comma and there is no whitespace inbetween.
