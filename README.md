# pyrtr

This package is a simple RTR daemon that can load VRPS from CSV and serve them to RTR clients.

It is useful mostly for testing and experiments with either advanced filtering engines that can
generate extended or filter actual RP-generated VRPS and for testing the RTR clients.

## Obtaining VRPS

The library expects the VRPS to be placed in the "current directory" when executed in file called `vrps.csv`.

To obtain fresh VRPS we recommend using [Routinator](https://github.com/NLnetLabs/routinator). When routinator is installed and configured the following command generates the VRPS CSV: `rouinator vrps > vrps.csv`.

Tetsing VRPS generated on 05/02/2023 are included in the repo.

## Installinga and running the pyrtr

```
poetry install
poetry run pyrtr
```

Then the RTR server loads the VRPS and starts listening on `0.0.0.0` on port `15432/tcp`.

## Compatibility

VRPS sources:
 * [Routinator](https://github.com/NLnetLabs/routinator)

RTR clients:
 * Cisco IOS XE version ???3???
 * BIRD 3.0.7

### BIRD test

BIRD config snippet used for testing:
```
roa4 table rtr4;
roa6 table rtr6;

protocol rpki pyrtr {
    debug all;
    roa4 { table rtr4; };
    roa6 { table rtr6; };
    remote "127.0.0.1" port 15432;
    retry keep 90;
    refresh keep 900;
    expire keep 172800;
}
```

