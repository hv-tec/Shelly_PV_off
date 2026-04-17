# Shelly_PV_off
NordPool negatiivne hind peatab PV inverteri tootmise Shelly relee abil
Telefonile saadetakse sõnum Pushover kaudu statuse muutuse ja NP hinna kohta

## Requirements

- Shelly device with scripting support (Gen2+, e.g. Shelly Plus 1, Pro 1, etc.)
- [Pushover](https://pushover.net) account (app token + user key)
- Internet access from Shelly device

## Who should use which configuration?

### Taastuvenergia toetusega tootjad (renewable energy support recipients)
```js
threshold_eur_kwh: 0,
tasakaalustus_eur_kwh: 0,  // jäta 0
marginaal_eur_kwh: 0,       // jäta 0
```

### Börsilepinguga tootjad (market price sellers)
Tootjad kes müüvad energiat börsihinnaga (nt Alexela börsipakett), saavad seadistada tegeliku müügihinna arvutuse — relee tõmbub kui efektiivne müügitulu langeb alla künnise.

```js
threshold_eur_kwh: 0,
tasakaalustus_eur_kwh: 0.00373,  // tasakaalustusteenus
marginaal_eur_kwh: 0.015,         // Alexela vm marginaal
```

## Configuration

Edit the `CFG` object at the top of the script:

| Parameter | Default | Unit | Description |
|---|---|---|---|
| `relay_id` | `0` | — | Shelly relay/switch ID (usually 0) |
| `threshold_eur_kwh` | `0` | €/kWh | Effective sell price below which relay activates. `0` = cut off only when price goes negative |
| `check_interval` | `120` | seconds | How often to fetch the current price |
| `api_timeout` | `15` | seconds | HTTP request timeout for Elering API and Pushover |
| `tasakaalustus_eur_kwh` | `0` | €/kWh | Balancing fee (tasakaalustusteenus), deducted from NP price. Default 0 |
| `marginaal_eur_kwh` | `0` | €/kWh | Broker sell margin (e.g. Alexela), deducted from NP price. Default 0 |
| `pushover_token` | `????` | — | Pushover application token |
| `pushover_user` | `????` | — | Pushover user key |

### Effective price formula
