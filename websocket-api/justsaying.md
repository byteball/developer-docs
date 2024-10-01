---
description: This is a message type, which doesn't require response back.
---

# JustSaying

Example:

```javascript
const network = require('ocore/network.js');
const eventBus = require('ocore/event_bus.js');
const ws = network.getInboundDeviceWebSocket(device_address);

// function parameters: websocket, subject, body
network.sendJustsaying(ws, 'custom', 'some data');

eventBus.on('custom_justsaying', function(ws, content) {
    console.log(content);
};
```

Following is a list of `justsaying` type JSON messages that are sent over the network:

### **Send version information**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'version', 
        body: {
            protocol_version: protocol_version, 
            alt: alt, 
            library: name, 
            library_version: version, 
            program: program, 
            program_version: program_version
        }
    }
}
```

### **Send free joints**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'free_joints_end', 
        body: null
    }
}
```

### **Send a private transaction**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'private_payment', 
        body: privateElement
    }
}
```

### **Share your node WebSocket URL to accept incoming connections**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'my_url', 
        body: my_url
    }
}
```

### **Ask to verify your WebSocket URL**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'want_echo', 
        body: random_echo_string
    }
}
```

### **Verify your WebSocket URL with echo message**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'your_echo', 
        body: echo_string
    }
}
```

### **Log in to Hub**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/login', 
        body: {
            challenge: challenge,
            pubkey: pubkey,
            signature: signature
        }
    }
}
```

### **Get new messages**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/refresh', 
        body: null
    }
}
```

### Remove handled message

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/delete', 
        body: message_hash
    }
}
```

### **Send pairing message**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/challenge', 
        body: challenge
    }
}
```

### **Send message to device**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/message', 
        body: {
            message_hash: message_hash,
            message: message
        }
    }
}
```

### Ask more messages

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/message_box_status', 
        body: 'has_more'
    }
}
```

### **Light wallet transaction update**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'light/have_updates', 
        body: null
    }
}
```

### **Light wallet sequence became bad**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'light/sequence_became_bad', 
        body: arrUniqueUnits
    }
}
```

### **Add light wallet to monitor address**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'light/new_address_to_watch', 
        body: address
    }
}
```

### **Send bug report**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'bugreport', 
        body: {
            message: message,
            exception: exception
        }
    }
}
```

### Push project number (only accepted from hub)

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'hub/push_project_number', 
        body: {
            projectNumber: projectNumber
        }
    }
}
```

### New version is available (only accepted from hub)

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'new_version', 
        body: {
            version: version
        }
    }
}
```

### **Exchange rates (only accepted from hub)**

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'exchange_rates', 
        body: exchangeRates
    }
}
```

### Ask to update (only accepted from hub)

```javascript
{
    type: 'justsaying',
    content: {
        subject: 'upgrade_required', 
        body: null
    }
}
```

### Watch system variables

```
{
    type: 'justsaying',
    content: {
        subject: 'watch_system_vars', 
        body: null
    }
}
```

This will subscribe you to updates of the currently active system variables and votes for them.

Immediately after this request, you'll get the current state of all system variables and their past values with the MCIs when they were voted in:

```
{
    type: 'justsaying',
    content: {
        subject: 'system_vars', 
        body: {
            op_list: [
                {
                    vote_count_mci: 123,
                    is_emergency: 0,
                    value: [
                        "2FF7PSL7FYXVU5UIQHCVDTTPUOOG75GX",
                        ....
                    ]
                },
                .....
            ],
            threshold_size: [
                {
                    vote_count_mci: 456,
                    is_emergency: 0,
                    value: 10000
                },
                .....
            ],
            .....    
        }
    }
}
```

Similar data will be pushed to you whenever any system variable changes as a result of vote count.

Also, user votes will be pushed to you as soon as they are sent by users (first, with `is_stable=0`, then with `is_stable=1` as soon as the voting transaction becomes final):

```
{
    type: "justsaying",
    content: {
        subject: "system_var_vote", 
        body: {
            subject: "op_list",
            value: [
                "2FF7PSL7FYXVU5UIQHCVDTTPUOOG75GX",
                ....
            ],
            author_addresses: ["EJC4A7WQGHEZEKW6RLO7F26SAR4LAQBU"],
            unit: "LpxhHEfxbOyj0sPlp6UC6XrRABvRJiH4qKEsOcMd1Bc=",
            is_stable: 1
        }
    }
}
```

### Custom JustSaying

You can add your own communication protocol on top of the Obyte one. See event [there](../list-of-events.md#event-for-custom-justsaying).

{% code title="" %}
```javascript
{
    type: 'justsaying',
    content: {
        tag: tag,
        subject: 'custom',
        body: body
    }
}
```
{% endcode %}
