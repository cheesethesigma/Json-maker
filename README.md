from http.server import BaseHTTPRequestHandler
import json

class handler(BaseHTTPRequestHandler):
    def do_GET(self):
        path = self.path.split("?")[0]

        routes = {
            "/v2/rpc/getWallet": {"currency": 1350},
            "/v2/rpc/getInventory": {
                "items": ["item_revolver_gold", "item_backpack", "item_flashlight"]
            },
            "/v2/rpc/getPromoReward": {
                "promo": "FREEJUNE",
                "reward_preview": "item_lootbox_rare",
                "redeemable": True
            },
            "/v2/rpc/getPlayerStats": {
                "kills": 27,
                "deaths": 4,
                "play_time_minutes": 138,
                "level": 5,
                "xp": 1820
            },
            "/v2/rpc/getLeaderboard": {
                "top_players": [
                    {"player": "Player1", "score": 5280},
                    {"player": "Player2", "score": 4710},
                    {"player": "Player3", "score": 4555}
                ]
            },
            "/v2/rpc/getServerStatus": {
                "status": "online",
                "uptime": "72 hours",
                "region": "NA-East"
            }
        }

        if path in routes:
            self.send_response(200)
            self.send_header("Content-Type", "application/json")
            self.end_headers()
            self.wfile.write(json.dumps(routes[path]).encode())
        else:
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b"Not Found")

    def do_POST(self):
        path = self.path
        content_length = int(self.headers.get('Content-Length', 0))
        post_data = self.rfile.read(content_length)
        data = json.loads(post_data or '{}')

        if path == "/v2/rpc/updateWalletSoftCurrency":
            response = {
                "new_balance": 1000 + int(data.get("amount", 0)),
                "status": "wallet updated"
            }
        elif path == "/v2/rpc/openLootbox":
            response = {
                "items": ["item_crate", "item_hat", "item_skin"],
                "message": "Lootbox opened"
            }
        else:
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b"Not Found")
            return

        self.send_response(200)
        self.send_header("Content-Type", "application/json")
        self.end_headers()
        self.wfile.write(json.dumps(response).encode())
