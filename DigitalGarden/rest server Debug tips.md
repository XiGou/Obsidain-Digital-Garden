# 鉴权：
curl -X POST -H "Content-Type: application/json" -d '{"username": "cli-user", "password": "!QAZ2wsx"}' 127.0.0.1/api/v3/sessions

curl -X GET -H "Content-Type: application/json" -H "X-SmartX-Token:0baecb7d2fe5417fa0af3ba63444a0e7" 172.20.237.129/api/v2/nfs/exports

curl -X POST -d  '{"name": "gouxi-test","replica_num": 2,"thin_provision": true,"description": "string"}' -H "Content-Type: application/json" -H "X-SmartX-Token:0baecb7d2fe5417fa0af3ba63444a0e7" 172.20.237.129/api/v2/nfs/exports

```python
import argparse
import requests
import concurrent.futures

def get_token(server_ip):
    url = f"http://{server_ip}/api/v3/sessions"
    payload = {
        "username": "cli-user",
        "password": "!QAZ2wsx"
    }
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        token = response.json().get("token")
        return token
    except (requests.RequestException, ValueError) as e:
        print("Failed to get token:", e)
        return None

def send_request(url, headers):
    try:
        response = requests.get(url, headers=headers)
        response.raise_for_status()
        return response.text
    except requests.RequestException as e:
        print(f"Failed to send request to {url}:", e)
        return ""

# 解析命令行参数
parser = argparse.ArgumentParser(description="Concurrent script for sending requests")
parser.add_argument("server_ip", help="Server IP address")
parser.add_argument("api_path", help="API path")
parser.add_argument("--concurrency", type=int, default=100, help="Concurrency level (default: 100)")
args = parser.parse_args()

server_ip = args.server_ip
api_path = args.api_path
concurrency = args.concurrency

token = get_token(server_ip)

if token:
    headers = {
        "Content-Type": "application/json",
        "X-SmartX-Token": token
    }

    urls = [f"http://{server_ip}{api_path}"] * concurrency

    with concurrent.futures.ThreadPoolExecutor(max_workers=concurrency) as executor:
        results = executor.map(send_request, urls, [headers] * len(urls))

    with open("zbs_rest_test.log", "w") as file:
        for result in results:
            file.write(result + "\n")
else:
    print("Failed to get token. Aborting script.")
```


