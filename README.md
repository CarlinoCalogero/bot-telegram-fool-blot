<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT HEADER -->
<br />
<div align="center">
  <h3 align="center">foolBlot</h3>

  <p align="center">
    A playful, serverless Telegram bot built on Cloudflare Workers and TypeScript for group chat member mentions, dynamic regex triggers, custom stickers, and inside jokes.
    <br />
    <a href="https://github.com/CarlinoCalogero/bot-telegram-fool-blot"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/CarlinoCalogero/bot-telegram-fool-blot/issues/new?labels=bug&template=bug-report---.md">Report Bug</a>
    &middot;
    <a href="https://github.com/CarlinoCalogero/bot-telegram-fool-blot/issues/new?labels=enhancement&template=feature-request---.md">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li>
      <a href="#usage">Usage</a>
      <ul>
        <li><a href="#bot-commands">Bot Commands</a></li>
        <li><a href="#keyword--regex-triggers">Keyword &amp; Regex Triggers</a></li>
      </ul>
    </li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

**foolBlot** is a serverless Telegram bot designed to bring humor, convenience, and interactive group engagement to Telegram chats. Deployed as a Cloudflare Worker, the bot operates at the edge with zero cold-start delay and no need for dedicated servers or always-on virtual machines.

Key capabilities include:
* **Targeted Squad Mentions:** Mention specific squads within the group (such as all admins, tech specialists, anime enthusiasts, meme makers, or manga readers) without spamming everyone.
* **Regex-Powered Triggers:** Passively listens for message patterns to automatically fire humorous replies, folded ASCII text memes (`mi piego`), barking copypastas, or meme audio/video clips.
* **Personalized Anime Reactions:** Detects sadness keywords (`sad`, `sadge`, `triste`, `tristezza`) and responds with custom anime character stickers specifically mapped to the user who sent the message (featuring characters like Kana Arima, Ruby Hoshino, Megumin, Aqua, Makise Kurisu, Ganyu, and Miko).
* **Edge-First Architecture:** Powered by Cloudflare Workers and TypeScript for speed, security, and scalability.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

* [![Cloudflare Workers][Cloudflare-shield]][Cloudflare-url]
* [![TypeScript][TypeScript-shield]][TypeScript-url]
* [![Wrangler][Wrangler-shield]][Wrangler-url]
* [![Telegram Bot API][Telegram-shield]][Telegram-url]
* [![Node.js][NodeJS-shield]][NodeJS-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

To set up a local development environment or deploy your own instance of foolBlot, follow these steps.

### Prerequisites

* **Node.js** (v18.x or later recommended) & **npm**
  ```sh
  npm install npm@latest -g
  ```
* **Cloudflare Account**: A free [Cloudflare](https://cloudflare.com) account to manage and deploy Workers.
* **Telegram Bot Token**: Create a bot with [@BotFather](https://t.me/botfather) on Telegram and keep your auth token handy.

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/CarlinoCalogero/bot-telegram-fool-blot.git
   cd bot-telegram-fool-blot
   ```

2. Install NPM dependencies:
   ```sh
   npm install
   ```

3. Configure your Telegram Bot secret token:
   * **For production deployment:** Store the secret encrypted in Cloudflare Workers:
     ```sh
     npx wrangler secret put TELEGRAM_AUTH_TOKEN
     ```
   * **For local development:** Create a `.dev.vars` file in the project root:
     ```env
     TELEGRAM_AUTH_TOKEN=your_telegram_bot_token_here
     ```

4. Run locally with Wrangler:
   ```sh
   npm run dev
   ```

5. Deploy the worker to Cloudflare:
   ```sh
   npm run deploy
   ```

6. Register the webhook with Telegram's API:
   ```sh
   curl -F "url=https://<your-worker-name>.<your-subdomain>.workers.dev" https://api.telegram.org/bot<YOUR_TELEGRAM_AUTH_TOKEN>/setWebhook
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

Once added to your group chat (with administrative privileges to read messages if group privacy mode is enabled), **foolBlot** responds to commands and monitors messages for trigger phrases.

### Bot Commands

You can configure these commands in `@BotFather` via `/setcommands`:

| Command | Description |
| :--- | :--- |
| `/admins` | Mentions all group administrators (excluding bots) |
| `/all` | Mentions all registered group members (filtering out blacklisted users) |
| `/animequery` | Calls forth the anime squad for recommendations and debate |
| `/techsquad` | Calls forth the tech elite squad to hear their thoughts |
| `/memequestion` | Summons the meme connoisseurs |
| `/askmanga` | Calls forth the manga readers to answer questions |
| `/codepain` | Pings the lead developer / CEO for urgent help |
| `/oppaiappraiser` | Calls upon the resident appraiser |

### Keyword & Regex Triggers

foolBlot automatically reacts to specific phrases and words in chat messages:

| Trigger Pattern | Action / Response |
| :--- | :--- |
| `mi piego`, `mi fa piegare`, `mi piegherò`, etc. | Sends personalized folded ASCII text for the sender |
| `ci pieghiamo`, `ci siamo piegati`, etc. | Sends folded ASCII text combining all group members |
| `yo` / `yoooo...` | Replies with the classic "YOOOOO" video clip |
| `miao` / `nya...` | Replies with the "MIAO" video clip |
| `sad`, `sadge`, `triste`, `tristezza` | Reacts with the sender's personalized anime sticker (Kana, Ruby, Megumin, Aqua, Kurisu, Ganyu, Miko) |
| `bruh` | Replies with the sad violin sticker |
| `woof`, `grr`, `bark`, `snarl`, `arf`, `awoo` | Unleashes the ferocious barking dog copypasta |

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

Please read [CONTRIBUTION.md](CONTRIBUTION.md) for full details on our code standards, project structure, and pull request workflow.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Top contributors:

<a href="https://github.com/CarlinoCalogero/bot-telegram-fool-blot/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=CarlinoCalogero/bot-telegram-fool-blot" alt="contrib.rocks image" />
</a>

<!-- CONTACT -->
## Contact

**Calogero Carlino**

* Email: [carlinocalogero.dev@gmail.com](mailto:carlinocalogero.dev@gmail.com)
* LinkedIn: [https://www.linkedin.com/in/calogero-carlino-2a94a33b5/](https://www.linkedin.com/in/calogero-carlino-2a94a33b5/)
* GitHub: [https://github.com/CarlinoCalogero](https://github.com/CarlinoCalogero)
* Portfolio: [https://carlinocalogero.github.io](https://carlinocalogero.github.io)

Project Link: [https://github.com/CarlinoCalogero/bot-telegram-fool-blot](https://github.com/CarlinoCalogero/bot-telegram-fool-blot)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)
* [Telegram Bot API Documentation](https://core.telegram.org/bots/api)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)
* [Shields.io](https://shields.io/)
* [Contrib.rocks](https://contrib.rocks/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/CarlinoCalogero/bot-telegram-fool-blot.svg?style=for-the-badge
[contributors-url]: https://github.com/CarlinoCalogero/bot-telegram-fool-blot/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/CarlinoCalogero/bot-telegram-fool-blot.svg?style=for-the-badge
[forks-url]: https://github.com/CarlinoCalogero/bot-telegram-fool-blot/network/members
[stars-shield]: https://img.shields.io/github/stars/CarlinoCalogero/bot-telegram-fool-blot.svg?style=for-the-badge
[stars-url]: https://github.com/CarlinoCalogero/bot-telegram-fool-blot/stargazers
[issues-shield]: https://img.shields.io/github/issues/CarlinoCalogero/bot-telegram-fool-blot.svg?style=for-the-badge
[issues-url]: https://github.com/CarlinoCalogero/bot-telegram-fool-blot/issues
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/calogero-carlino-2a94a33b5/
[Cloudflare-shield]: https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white
[Cloudflare-url]: https://workers.cloudflare.com/
[TypeScript-shield]: https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white
[TypeScript-url]: https://www.typescriptlang.org/
[Wrangler-shield]: https://img.shields.io/badge/Wrangler-F38020?style=for-the-badge&logo=cloudflare&logoColor=white
[Wrangler-url]: https://developers.cloudflare.com/workers/wrangler/
[Telegram-shield]: https://img.shields.io/badge/Telegram_Bot_API-26A5E4?style=for-the-badge&logo=telegram&logoColor=white
[Telegram-url]: https://core.telegram.org/bots/api
[NodeJS-shield]: https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white
[NodeJS-url]: https://nodejs.org/
