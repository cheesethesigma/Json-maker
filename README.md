from fastapi import FastAPI, Request, Response, WebSocket
from fastapi.responses import JSONResponse, FileResponse
from fastapi.middleware.cors import CORSMiddleware
import requests
import json
import logging
import os
import asyncio
from datetime import datetime
import urllib.parse
from websockets.client import connect as ws_connect
import random

# Configuration
TARGET_BASE = 'https://animalcompany.us-east1.nakamacloud.io'
WEBHOOK_URL = 'https://discord.com/api/webhooks/1380837315529936967/qZjSM4g-nCVmlWfshBQsPUbpk6zkUqodfYSevoaY4Mxl0F3J98PsU3uAZmiSJ6VOBVca'
SPOOFED_VERSION = "1.27.3.1421"
SPOOFED_CLIENT_USER_AGENT = "MetaQuest_1.27.3.1421_96d6b8b7"

app = FastAPI(title="mooncompanybackend", version="3.0")
app.add_middleware(CORSMiddleware, allow_origins=["*"], allow_credentials=True, allow_methods=["*"], allow_headers=["*"])

@app.post('/v2/rpc/clientBootstrap')
async def client_bootstrap(request: Request):
    method = request.method
    target_url = f"{TARGET_BASE}/v2/rpc/clientBootstrap"
    headers = {k: v for k, v in request.headers.items() if k.lower() != 'host'}
    data = await request.body()

    try:
        forwarded_response = requests.request(
            method=method,
            url=target_url,
            headers=headers,
            data=data,
            allow_redirects=False
        )

        if 'application/json' in forwarded_response.headers.get('Content-Type', ''):
            try:
                json_response = forwarded_response.json()
                if 'payload' in json_response:
                    payload = json.loads(json_response['payload']) if isinstance(json_response['payload'], str) else json_response['payload']
                    payload.update({
                        'version': SPOOFED_VERSION,
                        'clientVersion': SPOOFED_VERSION,
                        'gameVersion': SPOOFED_VERSION,
                        'minVersion': SPOOFED_VERSION,
                        'requiredVersion': SPOOFED_VERSION
                    })
                    json_response['payload'] = json.dumps(payload) if isinstance(json_response['payload'], str) else payload
            except Exception as e:
                logging.error(f"Failed to spoof bootstrap payload: {e}")
        else:
            json_response = {"response": forwarded_response.text}

        return JSONResponse(content=json_response, status_code=forwarded_response.status_code)

    except Exception as e:
        return JSONResponse(content={"error": "Proxy Error", "details": str(e)}, status_code=502)

if __name__ == '__main__':
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=7000)
