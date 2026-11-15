# Contributing to foolBlot

First off, thank you for considering contributing to **foolBlot**! 🎉

foolBlot is a lightweight, serverless Telegram bot built on Cloudflare Workers and TypeScript, designed to bring fun interactions, custom squad mentions, and automatic reactions into group chats. Whether you want to fix a bug, add a new squad mention command, introduce new trigger regexes, or improve documentation, your help is warmly welcomed.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Project Architecture](#project-architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Fork & Clone](#fork--clone)
  - [Environment Setup](#environment-setup)
  - [Local Development](#local-development)
- [How to Contribute](#how-to-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Features](#suggesting-features)
  - [Adding a New Command](#adding-a-new-command)
  - [Adding a Keyword / Regex Trigger](#adding-a-keyword--regex-trigger)
  - [Registering New Users or Stickers](#registering-new-users-or-stickers)
- [Code Style and Standards](#code-style-and-standards)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Contact](#contact)

---

## Code of Conduct

We are committed to providing a welcoming, inclusive, and harassment-free environment for everyone. Please be respectful, constructive, and open-minded when communicating in issues, discussions, and pull requests.

---

## Project Architecture

Understanding how the project is structured will help you navigate the codebase quickly:

```text
bot-telegram-fool-blot/
├── src/
│   ├── index.ts        # Main Cloudflare Worker fetch handler & message processing logic
│   ├── utils.ts        # User mappings, sticker IDs, ASCII art, and meme constants
│   └── types/          # TypeScript definitions matching Telegram Bot API objects
│       ├── ChatMember.ts
│       ├── ChatMemberAdministrator.ts
│       ├── MessageEntity.ts
│       ├── Update.ts
│       └── User.ts
├── .editorconfig       # Code editor style configurations (tabs, LF, UTF-8)
├── .prettierrc         # Prettier formatting rules
├── package.json        # NPM dependencies and scripts (dev, deploy)
├── tsconfig.json       # TypeScript compiler configuration
└── wrangler.toml       # Cloudflare Workers configuration
```

- **`src/index.ts`**: The entry point. Exporting `fetch(request, env, ctx)` handles incoming HTTP `POST` webhook payloads from Telegram. The function `processUpdate` inspects the message text and dispatches commands and regex triggers.
- **`src/utils.ts`**: Holds dictionary constants for user IDs (`USERS`, `USERS_ID`), folded-text ASCII representations (`MI_PIEGO`), video links (`YOOOOOOOOOOO`, `MIAO`), copypastas (`WOOF`), and Telegram sticker IDs (`STICKERS`).
- **`src/types/`**: Contains strong TypeScript typings for Telegram objects to ensure type safety without relying on bloated dependencies.

---

## Getting Started

### Prerequisites

Ensure you have the following installed on your machine:
- [Node.js](https://nodejs.org/) (v18.0.0 or later recommended)
- [npm](https://www.npmjs.com/) (bundled with Node.js)
- A Telegram account and a test bot created with [@BotFather](https://t.me/botfather)

### Fork & Clone

1. Fork the repository to your own GitHub account:
   [https://github.com/CarlinoCalogero/bot-telegram-fool-blot/fork](https://github.com/CarlinoCalogero/bot-telegram-fool-blot/fork)

2. Clone your fork locally:
   ```sh
   git clone https://github.com/<your-username>/bot-telegram-fool-blot.git
   cd bot-telegram-fool-blot
   ```

3. Install project dependencies:
   ```sh
   npm install
   ```

### Environment Setup

Create a `.dev.vars` file in the root of the project to supply your bot authentication token for local development:

```env
TELEGRAM_AUTH_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
```

> [!CAUTION]
> Never commit `.dev.vars` or any real API tokens to version control. The file is excluded by `.gitignore`.

### Local Development

Start the local Cloudflare Workers development server:

```sh
npm run dev
```

This starts Wrangler on `http://localhost:8787`. You can simulate webhook POST requests using tools like `curl` or Postman:

```sh
curl -X POST http://localhost:8787 \
  -H "Content-Type: application/json" \
  -d '{"update_id":1,"message":{"message_id":1,"chat":{"id":-123456789,"type":"group"},"from":{"id":873624965,"is_bot":false,"first_name":"Calogero"},"text":"bruh"}}'
```

---

## How to Contribute

### Reporting Bugs

- Check the [Issues tab](https://github.com/CarlinoCalogero/bot-telegram-fool-blot/issues) first to see if the bug has already been reported.
- If not, open a new issue with a descriptive title.
- Provide clear steps to reproduce the issue, including example messages sent to the bot, expected outcome, and the actual behavior observed.
- Include any relevant terminal output or error logs.

### Suggesting Features

- Open a feature request issue describing the proposed functionality.
- Explain the use case and why it would be beneficial for group interactions.

### Adding a New Command

1. Open `src/index.ts`.
2. Inside `processUpdate()`, locate the command handlers and add your command:
   ```ts
   if (lowerCaseUserText.startsWith('/newcommand')) {
   	const users = [
   		(await getChatMember(telegramAuthToken, chatId, USERS.CAL)).result.user,
   		// add additional target members...
   	];
   	const [message, messageEntities] = await multipleMentions(users);
   	await replyToMessage(telegramAuthToken, update.message, message, messageEntities);
   }
   ```
3. Update the command list in `README.md` and document it for BotFather.

### Adding a Keyword / Regex Trigger

1. Define a regular expression matching your target phrase (using case-insensitive and word-boundary flags when appropriate):
   ```ts
   const myCustomRegex: RegExp = /\b(?:customphrase)\b/gmi;
   if (myCustomRegex.test(lowerCaseUserText)) {
   	await replyToMessage(telegramAuthToken, update.message, 'Custom response text');
   }
   ```
2. If your trigger sends a media link, copypasta, or sticker, store the constant cleanly in `src/utils.ts`.

### Registering New Users or Stickers

1. Open `src/utils.ts`.
2. Add new user mappings under `USERS` and `USERS_ID`:
   ```ts
   export const USERS: Record<string, number> = {
   	// ...
   	NEW_USER: 123456789,
   };
   ```
3. Add sticker file IDs under `STICKERS`:
   ```ts
   export const STICKERS = {
   	// ...
   	my_sticker: 'CAACAg...',
   };
   ```

---

## Code Style and Standards

To maintain code consistency throughout the project:

- **Formatting**: The project uses Prettier and EditorConfig. Follow the existing configuration:
  - Indentation: **Tabs**
  - Quotes: **Single quotes** (`'...'`)
  - Semicolons: **Always**
  - Line width: **140 characters**
- **Type Checking**: Run TypeScript checks before submitting your changes to verify there are no compile errors:
  ```sh
  npx tsc --noEmit
  ```
- **Avoid `any`**: Use and expand the strongly-typed interfaces in `src/types/` whenever feasible.

---

## Submitting a Pull Request

Follow these steps to submit your contributions:

1. Create a dedicated branch for your changes:
   ```sh
   git checkout -b feature/my-new-feature
   # or
   git checkout -b fix/resolve-bug-issue
   ```

2. Make your changes and verify that:
   - TypeScript compiles cleanly (`npx tsc --noEmit`).
   - The code adheres to the project's formatting rules.
   - No sensitive information (such as tokens or personal secrets) is included.

3. Commit your changes with clear, descriptive commit messages:
   ```sh
   git commit -m "feat: add /gaming command for game squad mentions"
   ```

4. Push your branch to your GitHub fork:
   ```sh
   git push origin feature/my-new-feature
   ```

5. Open a **Pull Request** against the `main` branch of `CarlinoCalogero/bot-telegram-fool-blot`.
6. Provide a concise description of what changed, reference any relevant issue numbers (e.g., `Closes #4`), and wait for review!

---

## Contact

If you have any questions, feel free to reach out:

- **Calogero Carlino**
- Email: [carlinocalogero.dev@gmail.com](mailto:carlinocalogero.dev@gmail.com)
- LinkedIn: [https://www.linkedin.com/in/calogero-carlino-2a94a33b5/](https://www.linkedin.com/in/calogero-carlino-2a94a33b5/)
- GitHub: [@CarlinoCalogero](https://github.com/CarlinoCalogero)
