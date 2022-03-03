# Payload Design

Designed in a way to ensure same code can be used for both **gateway** as well as **sensor**.

_Payload from sensor can be directly passed as data for gateway payload and so on to higher levels._

This ensures that the code used to extract data is **reusable** at multiple levels.

## Gateway Payload

```json
{
    "device": {
        "type": "gateway",
        "id": "001"
    },
    "message": {
        "metadata": {
            "condition": {
                "bat_percentage": 86.7,
                "cycle_count": 226,
                "status": 1,
                "temp": 6,
                "time": 0,
                "voltage": 3.6
            },
            "settings": {
                "property": "value"
            }
        },
        "data_count": 2,
        "data_list": [
            "+++SENSOR_PAYLOAD_1+++",
            "+++SENSOR_PAYLOAD_2+++"
        ],
        "published_at": "2020-01-15T16:02:10.778Z"
    }
}
```

## Sensor Payload

```json
{
    "device": {
        "type": "sensor",
        "id": "sensor_id"
    },
    "message": {
        "metadata": {
            "condition": {
                "bat_percentage": 86.7,
                "cycle_count": 226,
                "status": 1,
                "temp": 6,
                "time": 0,
                "voltage": 3.6
            },
            "settings": {
                "property": "value"
            }
        },
        "data_count": 2,
        "data_list": [
            {
            "sensor_type": "Temperature",
            "value": 31,
            "unit": "C"
        },{
            "sensor_type": "Accelerometer",
            "value": "x: 0.0, y: 0.0, z: 0.0",
            "unit": "m/s^2"
        }
        ]
        "published_at": "2020-01-15T16:02:10.668Z"
    }
}
```

## Example

```javascript
 const sensor_payload = createPayload({
            type: "sensor",
            id: "0001",
        }, {
            bat_percentage: 86.7,
            cycle_count: 226,
            status: 1,
            temp: 6,
            time: 0,
            voltage: 3.6
        }, {
            property: "value"
        },
        [{
            sensor_type: "Accelerometer",
            value: {
                x: Math.random() * 2 - 1,
                y: Math.random() * 2 - 1,
                z: Math.random() * 4 + 7.801
            },
            unit: "m/s^2",
        }],
        new Date().toISOString()
    )

const gateway_payload = createPayload({
            type: "gateway",
            id: "01",
        }, {
            bat_percentage: 86.7,
            cycle_count: 226,
            status: 1,
            temp: 6,
            time: 0,
            voltage: 3.6
        }, {
            property: "value"
        }, [sensor_payload],
        new Date().toISOString(), )

device.publish(
    iot_topic,
    JSON.stringify(gateway_payload)
);

const createPayload = (device, condition, settings, data, published_at) => {
    return {
        device,
        message: {
            metadata: {
                condition,
                settings
            },
            data_count: data.length,
            data,
            published_at
        }
    }
}

```
