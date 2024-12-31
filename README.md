# Minecraft Server Proxy Using Docker Compose

[![Deployment Verification](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/00-deployment-verification.yml/badge.svg)](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions)

The badge displayed on my repository indicates the status of the deployment verification workflow as executed on the latest commit to the main branch.

**Passing**: This means the most recent commit has successfully passed all deployment checks, confirming that the Docker Compose setup functions correctly as designed.

📙 The complete installation guide is available on my [website](https://www.heyvaldemar.com/install-minecraft-server-proxy-using-docker-compose/).

📗 For details on deploying the Minecraft Server (not the proxy), check out this link: [Minecraft Server Using Docker Compose](https://github.com/heyvaldemar/minecraft-server-docker-compose/).

❗ Change variables in the `.env` to meet your requirements.

💡 Note that the `.env`, `velocity.toml` file, and `plugins` folder should be in the same directory as `minecraft-server-docker-compose.yml`.

Configure the Path to `velocity.toml` in `minecraft-server-docker-compose.yml`:

   - **Linux or macOS Users:**
     - Ensure the following line is active in your Docker Compose file to correctly mount the volume:

       ```yaml
       - ./velocity.toml:/config
       ```

   - **Windows Users:**
     - You need to adjust the volume mount path due to the different file path formatting in Windows. Uncomment the line below and comment out the previous Linux/macOS line:

       ```yaml
       - ./velocity.toml:/config/velocity.toml
       ```

   This adjustment will set the correct path format based on your operating system, ensuring the server configures properly.

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

## Author

hey everyone,

💾 I’ve been in the IT game for over 20 years, cutting my teeth with some big names like [IBM](https://www.linkedin.com/in/heyvaldemar/), [Thales](https://www.linkedin.com/in/heyvaldemar/), and [Amazon](https://www.linkedin.com/in/heyvaldemar/). These days, I wear the hat of a DevOps Consultant and Team Lead, but what really gets me going is Docker and container technology - I’m kind of obsessed!

💛 I have my own IT [blog](https://www.heyvaldemar.com/), where I’ve built a [community](https://discord.gg/AJQGCCBcqf) of DevOps enthusiasts who share my love for all things Docker, containers, and IT technologies in general. And to make sure everyone can jump on this awesome DevOps train, I write super detailed guides (seriously, they’re foolproof!) that help even newbies deploy and manage complex IT solutions.

🚀 My dream is to empower every single person in the DevOps community to squeeze every last drop of potential out of Docker and container tech.

🐳 As a [Docker Captain](https://www.docker.com/captains/vladimir-mikhalev/), I’m stoked to share my knowledge, experiences, and a good dose of passion for the tech. My aim is to encourage learning, innovation, and growth, and to inspire the next generation of IT whizz-kids to push Docker and container tech to its limits.

Let’s do this together!

## My 2D Portfolio

🕹️ Click into [sre.gg](https://www.sre.gg/) — my virtual space is a 2D pixel-art portfolio inviting you to interact with elements that encapsulate the milestones of my DevOps career.

## My Courses

🎓 Dive into my [comprehensive IT courses](https://www.heyvaldemar.com/courses/) designed for enthusiasts and professionals alike. Whether you're looking to master Docker, conquer Kubernetes, or advance your DevOps skills, my courses provide a structured pathway to enhancing your technical prowess.

🔑 [Each course](https://www.udemy.com/user/heyvaldemar/) is built from the ground up with real-world scenarios in mind, ensuring that you gain practical knowledge and hands-on experience. From beginners to seasoned professionals, there's something here for everyone to elevate their IT skills.

## My Services

💼 Take a look at my [service catalog](https://www.heyvaldemar.com/services/) and find out how we can make your technological life better. Whether it's increasing the efficiency of your IT infrastructure, advancing your career, or expanding your technological horizons — I'm here to help you achieve your goals. From DevOps transformations to building gaming computers — let's make your technology unparalleled!

## Patreon Exclusives

🏆 Join my [Patreon](https://www.patreon.com/heyvaldemar) and dive deep into the world of Docker and DevOps with exclusive content tailored for IT enthusiasts and professionals. As your experienced guide, I offer a range of membership tiers designed to suit everyone from newbies to IT experts.

## My Recommendations

📕 Check out my collection of [essential DevOps books](https://kit.co/heyvaldemar/essential-devops-books)\
🖥️ Check out my [studio streaming and recording kit](https://kit.co/heyvaldemar/my-studio-streaming-and-recording-kit)\
📡 Check out my [streaming starter kit](https://kit.co/heyvaldemar/streaming-starter-kit)

## Follow Me

🎬 [YouTube](https://www.youtube.com/channel/UCf85kQ0u1sYTTTyKVpxrlyQ?sub_confirmation=1)\
🐦 [X / Twitter](https://twitter.com/heyvaldemar)\
🎨 [Instagram](https://www.instagram.com/heyvaldemar/)\
🐘 [Mastodon](https://mastodon.social/@heyvaldemar)\
🧵 [Threads](https://www.threads.net/@heyvaldemar)\
🎸 [Facebook](https://www.facebook.com/heyvaldemarFB/)\
🧊 [Bluesky](https://bsky.app/profile/heyvaldemar.bsky.social)\
🎥 [TikTok](https://www.tiktok.com/@heyvaldemar)\
💻 [LinkedIn](https://www.linkedin.com/in/heyvaldemar/)\
📣 [daily.dev Squad](https://app.daily.dev/squads/devopscompass)\
🧩 [LeetCode](https://leetcode.com/u/heyvaldemar/)\
🐈 [GitHub](https://github.com/heyvaldemar)

## Community of IT Experts

👾 [Discord](https://discord.gg/AJQGCCBcqf)

## Refill My Coffee Supplies

💖 [PayPal](https://www.paypal.com/paypalme/heyvaldemarCOM)\
🏆 [Patreon](https://www.patreon.com/heyvaldemar)\
💎 [GitHub](https://github.com/sponsors/heyvaldemar)\
🥤 [BuyMeaCoffee](https://www.buymeacoffee.com/heyvaldemar)\
🍪 [Ko-fi](https://ko-fi.com/heyvaldemar)

🌟 **Bitcoin (BTC):** bc1q2fq0k2lvdythdrj4ep20metjwnjuf7wccpckxc\
🔹 **Ethereum (ETH):** 0x76C936F9366Fad39769CA5285b0Af1d975adacB8\
🪙 **Binance Coin (BNB):** bnb1xnn6gg63lr2dgufngfr0lkq39kz8qltjt2v2g6\
💠 **Litecoin (LTC):** LMGrhx8Jsx73h1pWY9FE8GB46nBytjvz8g

<div align="center">

### Show some 💜 by starring some of the [repositories](https://github.com/heyValdemar?tab=repositories)!

![octocat](https://user-images.githubusercontent.com/10498744/210113490-e2fad07f-4488-4da8-a656-b9abbdd8cb26.gif)

</div>

![footer](https://user-images.githubusercontent.com/10498744/210157572-1fca0242-8af2-46a6-bfa3-666ffd40ebde.svg)
