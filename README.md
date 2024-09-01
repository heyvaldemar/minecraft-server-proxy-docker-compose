# Minecraft Server Proxy Using Docker Compose

📙 The complete installation guide is available on my [website](https://www.heyvaldemar.com/install-minecraft-server-proxy-using-docker-compose/).

📗 For details on deploying the Minecraft Server (not the proxy), check out this link: [Minecraft Server Using Docker Compose](https://github.com/heyvaldemar/minecraft-server-docker-compose/).

❗ Change variables in the `.env` to meet your requirements.

💡 Note that the `.env`, `velocity.toml` files, and `plugins` folder should be in the same directory as `minecraft-server-docker-compose.yml`.

❗ Ensure that you connect to the same Docker network you used when deploying your Minecraft servers with Docker Compose.

❗ Adjust the path to `velocity.toml` in the `minecraft-server-docker-compose.yml` based on your operating system:

- **For Linux or macOS:**
  - Uncomment the following line and comment out the `- ./velocity.toml:/config/velocity.toml` line:

    ```yaml
    - ./velocity.toml:/config
    ```

This adjusts the volume mount path to the appropriate location for Linux or macOS environments.

- **For Windows:**
  - Ensure this line is active in your Docker Compose file:

    ```yaml
    - ./velocity.toml:/config/velocity.toml
    ```

This sets the correct path format for Windows environments.

## Configuration Requirements for the Proxy Server

To ensure the proxy server functions correctly, update both the IP addresses and ports in the `velocity.toml` file to reflect your specific Minecraft server setup. It’s crucial to replace the default `127.0.0.1` IP and example ports (25580, 25581) with the actual server IPs and the ports on which your Minecraft Server is running:

```toml
[servers]
# Configure your servers here.
lobby = "127.0.0.1:25580"
survival = "127.0.0.1:25581"

# The order in which servers are tried when a player logs in or is kicked from a server.
try = [
    "lobby",
    "survival"
]
```

## Initial Server Setup

Upon the first startup of the Minecraft Proxy, a `forwarding.secret` file will be generated. Enter the contents of this file into the `config/paper-global.yml` on the Minecraft Server (not the proxy) within the `velocity` section to ensure a secure connection:

```yaml
velocity:
  enabled: true
  online-mode: true
  secret: REPLACE_WITH_CONTENT_FROM_THE_forwarding.secret_FILE
```

## Switching Between Servers

Players can navigate between different Minecraft servers using simple console commands or through an interactive lobby interface provided by the [Phoenix Lobby](https://www.myphoenixstore.com/en/items/phoenix-lobby) plugin. For command-line switching, simply type `/server survival` to connect directly to the 'survival' server. Alternatively, for a more visually engaging experience, consider installing the Phoenix Lobby plugin on a separate server. This plugin enhances the lobby with customizable options like server selectors and NPCs, making server transitions smooth and visually appealing.

This approach not only meets functional requirements but also offers a visually appealing and user-friendly environment for players.

Deploy Minecraft Server using Docker Compose:

`docker compose -f minecraft-server-proxy-docker-compose.yml -p minecraft-server-proxy up -d`

# Author

I’m Vladimir Mikhalev, the [Docker Captain](https://www.docker.com/captains/vladimir-mikhalev/), but my friends can call me Valdemar.

🌐 My [website](https://www.heyvaldemar.com/) with detailed IT guides\
🎬 Follow me on [YouTube](https://www.youtube.com/channel/UCf85kQ0u1sYTTTyKVpxrlyQ?sub_confirmation=1)\
🐦 Follow me on [Twitter](https://twitter.com/heyValdemar)\
🎨 Follow me on [Instagram](https://www.instagram.com/heyvaldemar/)\
🧵 Follow me on [Threads](https://www.threads.net/@heyvaldemar)\
🐘 Follow me on [Mastodon](https://mastodon.social/@heyvaldemar)\
🧊 Follow me on [Bluesky](https://bsky.app/profile/heyvaldemar.bsky.social)\
🎸 Follow me on [Facebook](https://www.facebook.com/heyValdemarFB/)\
🎥 Follow me on [TikTok](https://www.tiktok.com/@heyvaldemar)\
💻 Follow me on [LinkedIn](https://www.linkedin.com/in/heyvaldemar/)\
🐈 Follow me on [GitHub](https://github.com/heyvaldemar)

# Communication

👾 Chat with IT pros on [Discord](https://discord.gg/AJQGCCBcqf)\
📧 Reach me at ask@sre.gg

# Give Thanks

💎 Support on [GitHub](https://github.com/sponsors/heyValdemar)\
🏆 Support on [Patreon](https://www.patreon.com/heyValdemar)\
🥤 Support on [BuyMeaCoffee](https://www.buymeacoffee.com/heyValdemar)\
🍪 Support on [Ko-fi](https://ko-fi.com/heyValdemar)\
💖 Support on [PayPal](https://www.paypal.com/paypalme/heyValdemarCOM)
