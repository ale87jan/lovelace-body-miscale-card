# body-miscale-card

[![GH-release](https://img.shields.io/github/v/release/dckiller51/lovelace-body-miscale-card.svg?style=flat-square)](https://github.com/ale87jan/lovelace-body-miscale-card/releases)
[![GH-downloads](https://img.shields.io/github/downloads/dckiller51/lovelace-body-miscale-card/total?style=flat-square)](https://github.com/ale87jan/lovelace-body-miscale-card/releases)
[![GH-last-commit](https://img.shields.io/github/last-commit/dckiller51/lovelace-body-miscale-card.svg?style=flat-square)](https://github.com/ale87jan/lovelace-body-miscale-card/commits/main)
[![GH-code-size](https://img.shields.io/github/languages/code-size/dckiller51/lovelace-body-miscale-card.svg?color=red&style=flat-square)](https://github.com/ale87jan/lovelace-body-miscale-card)
[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg?style=flat-square)](https://github.com/hacs)

Card for data of Xiaomi scales in the Lovelace user interface of Home Assistant

The card is linked to the Bodymiscale custom components for Xiaomi scales. <https://github.com/ale87jan/lovelace-body-miscale-card> based on <https://github.com/dckiller51/bodymiscale>

## Installation

Manually add [body-miscale-card.js](https://raw.githubusercontent.com/dckiller51/lovelace-body-miscale-card/master/dist/body-miscale-card.js)
to your `<config>/www/` folder and add the following to the `configuration.yaml` file:

```yaml
lovelace:
  resources:
    - url: /local/body-miscale-card.js?v=1.0.0
      type: module
```

_OR_ install using [HACS](https://hacs.xyz/) and add this (if in YAML mode):

```yaml
lovelace:
  resources:
    - url: /hacsfiles/lovelace-body-miscale-card/body-miscale-card.js
      type: module
```

The above configuration can be managed directly in the Configuration -> Lovelace Dashboards -> Resources panel when not using YAML mode,
or added by clicking the "Add to lovelace" button on the HACS dashboard after installing the plugin.

If you want to use the scales background image, download and add
[src/images/miscale2.jpg](https://raw.githubusercontent.com/dckiller51/lovelace-body-miscale-card/master/src/images/miscale2.jpg)
to `<config>/www/images/` or configure your own preferred path.

For body score icons, download and add
[src/images/bodyscoreIcon/*.png](https://raw.githubusercontent.com/dckiller51/lovelace-body-miscale-card/master/src/images/bodyscoreIcon)
to `<config>/www/images/bodyscoreIcon/`.

## Configuration

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| type | string | **Required** | `custom:body-miscale-card`
| entity | string | **Required** | `bodymiscale.name`
| name | string/bool | `friendly_name` | Override friendly name (set to `false` to hide)
| image | string/bool | `false` | Set path/filename of background image (i.e. `/local/img/miscale2.jpg`)
| attributes | [Entity Data](#entity data) | *(see below)* | Set to `false` to hide all attributes
| buttons | [Button Data](#button data) | *(see below)* | Set to `false` to hide button row

### Entity Data

Default bodymiscale attributes under each list:

- `attributes` (**right list**) include `weight`, `impedance` (Optional), `height`, `age` and `gender`.
- `body` (**below list**) include `water` (miscale 181B), `visceral_fat`, `body_fat` (miscale 181B), `bmi`, `muscle_mass` (miscale 181B),
                                 `protein` (miscale 181B), `basal_metabolism`, `bone_mass` (miscale 181B), `metabolic_age` (miscale 181B),
                                 `ideal`, `body_type`.

See [examples](#examples) on how to customize, hide or add custom attributes.

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| key | string | **Required** | Attribute key on bodymiscale entity
| icon | string | | Optional icon
| label | string | | Optional label text
| unit | string | | Optional unit

### Button Data

Default buttons include `user1`, `user2`, `user3`, `user4` and `user5`.
See [examples](#examples) on how to customize, hide or add custom buttons/actions.

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| icon | string | **Required** | Show or hide stop button
| service | string | **Required** | Service to call (`input_boolean.toggle`)
| show | bool | `true` | Show or hide button
| label | string | | Optional label on hover
| service_data | object | | entity_id: input_boolean.bodyscale_user1_info_toggle

### Other models

Define your model. `false` (181D) or `true` (181B) (with to impedance)

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| model | bool | `false` | Supported models: with or without impedance

## Screenshots

![body-miscale-card](https://raw.githubusercontent.com/dckiller51/lovelace-body-miscale-card/master/examples/add-card.png)

![body-miscale-card-image](https://raw.githubusercontent.com/dckiller51/lovelace-body-miscale-card/master/examples/card-configuration.png)

## Examples

Basic configuration:

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
```

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  image: /local/custom/folder/background.jpg
  name: My Bodymiscale
  model: false
```

Hide specific attributes and/or buttons:

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  attributes:
    age: false
    gender: false
  buttons:
    user1: false
```

Customize specific buttons:

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  buttons:
    user1:
      icon: 'mdi:alpha-a-circle'
      label: Aurélien
      service_data:
        entity_id: input_boolean.bodyscale_aurelien_info_toggle
```

Add custom attributes:

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  attributes:
    body_type:
      key: body_type
      label: 'Body type: '
    water:
      key: water
      label: 'Eau: '
      unit: '%'
```

Add custom buttons and service calls:

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  buttons:
    new_button:
      icon: mdi:light-switch
      label: Custom button!
      service: light.turn_off
      service_data:
        entity_id: light.living_room
```

Add custom bar options (To know the start, destination, color and target values, open your Mi Fit app on your smartphone.)

## Options

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| color | string | var(--score-card-color, var(--ha-card-background)) | Color of the bar.
| height | string | 30px | Defines the height of the bar.
| max | number | 100 | Defines maximum value of the bar.
| min | number | 0 | Defines minimum value of the bar.
| positions | object | none | Defines the positions of the card elements. See [Positions Options](#positions-options).
| severity | object | none | A list of severity values. See [Severity Options](#severity-options).
| target | number | none | Defines and enables target marker value.
| width | string | 100% | Defines the width of the bar (**Required** the name must be on `outside`).

## Severity Options

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| from | number | **Required** | Defines from which value the color should be displayed.
| to | number | **Required** | Defines to which value the color should be displayed.
| color | string | **Required** | Defines the color to be displayed.

## Positions Options

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| icon | string | outside | `inside`, `outside`, `off`
| name | string | inside | `inside`, `outside`, `off`
| minmax | string | off | `inside`, `outside`, `off`
| value | string | inside | `inside`, `outside`, `off`

```yaml
type: custom:body-miscale-card
entity: bodymiscale.test
model: false
show_name: true
show_states: true
show_attributes: true
show_toolbar: true
show_body: true
show_buttons: true
entity_row: true
buttons:
  user1:
    show: true
body:
  bmi:
    positions:
      name: outside
      minmax: inside
    width: 50%
    target: 21
  bmi_label:
    color: blue
    height: 40px
  visceral_fat:
    severity:
      - from: 0
        to: 4.99
        color: blue
      - from: 5
        to: 10
        color: green
      - from: 10.01
        to: 15
        color: red
    target: 15
    min: 0
    max: 25
```

Translations: Automatic (setting of your homeassistant) or manual
Currently the languages available are `DE`,`EN`,`FR`,`IT`,`NL`,`PT-BR`,`ZH-HANS`, you can contact me to integrate your native language

```yaml
- type: custom:body-miscale-card
  entity: bodymiscale.name
  body:
    water:
      label: 'Eau: '
    bmi:
      label: 'IMC: '
  attributes:
    weight:
      label: 'Poids: '
      unit: ' kg'
    height:
      label: 'Taille: '
      unit: ' cm'
    age:
      label: 'Age: '
      unit: ' ans'
    gender:
      label: 'Genre: '
  buttons:
    user1:
      label: Aurélien
    user2:
      label: Siham
      show: true
```

## Credits

The card is based on the work of Ben Tomlin <https://github.com/benct/lovelace-xiaomi-vacuum-card>
The card is based on the work of Denys Dovhan <https://github.com/denysdovhan/purifier-card>

## Disclaimer

This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with the Xiaomi Corporation,
or any of its subsidiaries or its affiliates. The official Xiaomi website can be found at <https://www.mi.com/global/>.
