import time
import json
import urllib.request
import urllib.error
from datetime import datetime

def check_website(url):
    start = time.time()

    try:
        request = urllib.request.Request(
            url,
            headers={"User-Agent": "WebsiteMonitor/1.0"}
        )

        with urllib.request.urlopen(request, timeout=10) as response:
            status = response.status

    except urllib.error.HTTPError as error:
        status = error.code

    except Exception as error:
        return {
            "url": url,
            "status": "ERROR",
            "response_time": None,
            "error": str(error)
        }

    elapsed = round(time.time() - start, 3)

    return {
        "url": url,
        "status": status,
        "response_time": elapsed,
        "checked_at": datetime.now().isoformat()
    }

def main():
    websites = [
        "https://github.com",
        "https://google.com",
        "https://python.org"
    ]

    results = []

    for website in websites:
        print(f"Checking {website}...")
        result = check_website(website)
        results.append(result)

        print(result)

    with open("website_report.json", "w") as file:
        json.dump(results, file, indent=4)

    print("\nReport saved successfully!")

if __name__ == "__main__":
    main()
