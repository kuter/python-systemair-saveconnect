# python-systemair-saveconnect

This is a simple package that expose the SaveConnect API as a python module.

# Installation
```python
pip install git+https://github.com/kuter/python-systemair-saveconnect.git
```

# Example

```python
import asyncio

import requests

from systemair.saveconnect import SaveConnect
from systemair.saveconnect.graphql import SaveConnectGraphQL

email = ""
password = ""

api = SaveConnect(
    email=email,
    password=password,
    ws_enabled=True,
    update_interval=60,
    refresh_token_interval=300
)

async def login():
    # Authenticate
    if not await api.login():
        raise RuntimeError("Could not connect to systemAIR")

loop = asyncio.get_event_loop()
try:
    loop.run_until_complete(login())
except RuntimeError:
    raise
finally:
    loop.close()

obj = SaveConnectGraphQL(api)
obj.set_access_token(sc.auth.token)

url = obj.api_url
token = sc.auth.token


query = """
{
  GetAccountDevices {
    identifier
    name
    street
    zipcode
    city
    country
    deviceType {
      entry
      module
      scope
      type
    }
    status {
      connectionStatus
      serialNumber
      model
      startupWizardRequired
      updateInProgress
      filterLocked
      weekScheduleLocked
      serviceLocked
      hasAlarms
      units {
        temperature
        pressure
        flow
      }
    }
  }
}
"""

headers = {
    "content-type": "application/json",
    "x-access-token": token
}
print(headers)
response = requests.post(url, headers=headers, json=dict(query=query))

# # Refresh Token
# await sc.auth.refresh_token()
#
# devices = await sc.get_devices()
# device_0 = devices[0]["identifier"]
#
# device_0_data = await sc.read_data(device_id=device_0)

```

# Version History
* WIP   - fetch for fetching devices
* 3.0.0 - Updated to work with SaveConnect
* 1.0.0 - Initial Version
