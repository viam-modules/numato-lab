# [`numato-lab` module](https://github.com/viam-modules/numato-lab)

This [numato-lab module](https://app.viam.com/module/viam/numato-lab) implements a [Numato-lab GPIO Peripheral Modules](https://numato.com/product-category/automation/gpio-modules/) using the [`rdk:component:board` API](https://docs.viam.com/appendix/apis/components/board/).

> [!NOTE]
> Before configuring your board, you must [create a machine](https://docs.viam.com/cloud/machines/#add-a-new-machine).

Navigate to the [**CONFIGURE** tab](https://docs.viam.com/configure/) of your [machine](https://docs.viam.com/fleet/machines/) in the [Viam app](https://app.viam.com/).
[Add board / numato-lab:usb-gpio to your machine](https://docs.viam.com/configure/#components).

## Configure your usb-gpio board

On the new component panel, copy and paste the following attribute template into your board's attributes field:

```json
{
  "pins": <number>,
}
```

### Attributes

The following attributes are available for `viam:numato-lab:usb-gpio` boards:

| Attribute | Type | Required? | Description |
| --------- | ---- | --------- | ----------  |
| `pins` | int | **Required** | Number of GPIO pins available on the module. |
| `analogs` | object | Optional | Attributes of any pins that can be used as Analog-to-Digital Converter (ADC) inputs. |

For instructions on implementing analogs, see [Analogs configuration](#Analogs-configuration).

### Example configuration

### `viam:numato-lab:usb-gpio`
```json
  {
      "name": "<your-numato-lab-usb-gpio-board-name>",
      "model": "viam:numato-lab:usb-gpio",
      "type": "board",
      "namespace": "rdk",
      "attributes": {
      },
      "depends_on": []
  }
```

### Analogs configuration
An [analog-to-digital converter](https://www.electronics-tutorials.ws/combination/analogue-to-digital-converter.html) (ADC) takes a continuous voltage input (analog signal) and converts it to an discrete integer output (digital signal).

To integrate an ADC into your machine, you must first physically connect the pins on your ADC to your board.

Then, integrate `analogs` into your board by adding the following to your board's `attributes` configuration:

```json {class="line-numbers linkable-line-numbers"}
"analogs": [
  {
    "name": "<your-analog-reader-name>",
    "pin": "<pin-number-on-adc>",
    "spi_bus": "<your-spi-bus-index>",
    "chip_select": "<chip-select-index>",
    "average_over_ms": <int>,
    "samples_per_sec": <int>
  }
]
```

### Attributes

The following attributes are available for `analogs`:

| Name | Type | Required? | Description |
| ---- | ---- | --------- | ----------- |
|`name` | string | **Required** | Your name for the analog reader. |
|`pin`| string | **Required** | The pin number of the ADC's connection pin, wired to the board. This should be labeled as the physical index of the pin on the ADC.
|`chip_select`| string | **Required** | The chip select index of the board's connection pin, wired to the ADC. |
|`spi_bus` | string | **Required** | The index of the SPI bus connecting the ADC and board. |
| `average_over_ms` | int | Optional | Duration in milliseconds over which the rolling average of the analog input should be taken. |
|`samples_per_sec` | int | Optional | Sampling rate of the analog input in samples per second. |

### Example configuration

```json {class="line-numbers linkable-line-numbers"}
{
  "components": [
    {
      "name": "<your-numato-lab-board-name>",
      "model": "viam:numato-lab:usb-gpio",
      "type": "board",
      "namespace": "rdk",
      "attributes": {
        "analogs": [
          {
            "name": "current",
            "pin": "1",
            "spi_bus": "1",
            "chip_select": "0"
          },
          {
            "name": "pressure",
            "pin": "0",
            "spi_bus": "1",
            "chip_select": "0"
          }
        ]
      }
    }
  ]
}
```


### Next Steps
- To test your board, expand the **TEST** section of its configuration pane or go to the [**CONTROL** tab](https://docs.viam.com/fleet/control/).
- To write code against your board, use one of the [available SDKs](https://docs.viam.com/sdks/).
- To view examples using a board component, explore [these tutorials](https://docs.viam.com/tutorials/).
