# Test
Just a Test
import requests
import pandas as pd
import time

HEADERS = {
    "User-Agent": "AddressGeocoder/1.0"
}

def geocode(address):
    url = "https://nominatim.openstreetmap.org/search"

    params = {
        "q": address,
        "format": "jsonv2",
        "limit": 1
    }

    try:
        response = requests.get(
            url,
            params=params,
            headers=HEADERS,
            timeout=30
        )

        response.raise_for_status()

        data = response.json()

        if not data:
            return None, None

        return (
            float(data[0]["lat"]),
            float(data[0]["lon"])
        )

    except Exception as e:
        print(f"Error: {address}")
        print(e)
        return None, None


with open("addresses.txt", encoding="utf-8") as f:
    addresses = [
        line.strip()
        for line in f
        if line.strip()
    ]

results = []

for address in addresses:

    lat, lon = geocode(address)

    results.append({
        "Address": address,
        "Latitude": lat,
        "Longitude": lon
    })

    print(
        f"{address} -> "
        f"{lat}, {lon}"
    )

    time.sleep(1.1)

df = pd.DataFrame(results)

df.to_csv(
    "coordinates.csv",
    index=False
)

print("\nFinished!")
print("Saved: coordinates.csv")
Geolocation
