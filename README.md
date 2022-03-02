# Payload Design

## Gateway Payload

```json
{
    "device": {
        "type": "gateway",
        "id": "gateway_id"
    },
    "message": {
        "metadata": {
            "condition": {
                "bat_percentage": 86.66666666666667,
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
        "data": "sensor_payload",
        "published_at": "2020-01-15T16:02:10.778892406Z"
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
                "bat_percentage": 86.66666666666667,
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
        "data": {
            "sensor_type": "Temperature or Accelerometer",
            "value": "6 or 'x: 0.0, y: 0.0, z: 0.0'",
            "unit": "C or m/s^2"
        },
        "published_at": "2020-01-15T16:02:10.668Z"
    }
}
```

## How to implement

```javascript
 const sensor_payload = createPayload({
            type: "sensor",
            id: "0001",
        }, {
            bat_percentage: 86.66666666666667,
            cycle_count: 226,
            status: 1,
            temp: 6,
            time: 0,
            voltage: 3.6
        }, {
            property: "value"
        }, {
            sensor_type: "Accelerometer",
            value: {
                x: Math.random() * 2 - 1,
                y: Math.random() * 2 - 1,
                z: Math.random() * 4 + 7.801
            },
            unit: "m/s^2",
        },
        new Date().toISOString()
    )

const gateway_payload = createPayload({
            type: "gateway",
            id: "01",
        }, {
            bat_percentage: 86.66666666666667,
            cycle_count: 226,
            status: 1,
            temp: 6,
            time: 0,
            voltage: 3.6
        }, {
            property: "value"
        }, sensor_payload,
        new Date().toISOString(), )

device.publish(
    iot_topic,
    JSON.stringify(gateway_payload)
);

const createPayload = (device, condition, settings, data, published_at) => {
    {
        device,
        message: {
            metadata: {
                condition,
                settings
            },
            data,
            published_at
        }
    }
}

```
