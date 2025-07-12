![Launch On Bonk Banner](./image.jpg)

# 🚀 Launch On Bonk

Launch On Bonk makes launching your own coin on X effortless and accessible.

🔗 [Check out our website → launchonbonk.online](https://launchonbonk.online/)

---

## 🌟 Features

- No Fees  
  → ZERO trading fees

- Built on Solana  
  → Fast transactions, low fees, and high scalability.

- Fast  
  → The fastest way to launch a coin in all of crypto

- Rewards  
  → Releasing a reward system that incentivizes coin longevity

---
Got it — here's a **Discord bot script using slash commands (interactions)** and mirroring the **original 5-step flow** for documentation clarity. This version is structured to match your documentation format exactly, with clearly separated steps and inline comments to match each stage.

---

## `bot.ts` – Discord Bot Using Interactions (Discord.js v14, TypeScript)

```ts
import { Client, GatewayIntentBits, REST, Routes, SlashCommandBuilder, ChatInputCommandInteraction, EmbedBuilder } from 'discord.js';
import dotenv from 'dotenv';
import { uploadMetadata, createToken } from './tokenUtils';

dotenv.config();

const client = new Client({
  intents: [GatewayIntentBits.Guilds],
});

const DISCORD_BOT_TOKEN = process.env.DISCORD_BOT_TOKEN!;
const CLIENT_ID = process.env.CLIENT_ID!;
const GUILD_ID = process.env.GUILD_ID!;
const CHANNEL_ID = process.env.TARGET_CHANNEL_ID!; 

```
### 1. Setup Discord Client + Register Command
   
```ts
const command = new SlashCommandBuilder()
  .setName('launch')
  .setDescription('Launch a token on LaunchOnBonk')
  .addStringOption(opt =>
    opt.setName('symbol')
      .setDescription('Token symbol (e.g., BONK)')
      .setRequired(true))
  .addStringOption(opt =>
    opt.setName('name')
      .setDescription('Token name (e.g., Bonk Token)')
      .setRequired(true));

const rest = new REST({ version: '10' }).setToken(DISCORD_BOT_TOKEN);

async function registerCommands() {
  await rest.put(Routes.applicationGuildCommands(CLIENT_ID, GUILD_ID), {
    body: [command.toJSON()],
  });
  console.log('✅ Slash command registered');
}
```
### 2. Listen for Interaction Command
   
```ts
client.on('interactionCreate', async (interaction) => {
  if (!interaction.isChatInputCommand() || interaction.commandName !== 'launch') return;
  await handleLaunch(interaction);
});
```
### 3. Upload Metadata + Create Token
   
```ts
async function handleLaunch(interaction: ChatInputCommandInteraction) {
  const symbol = interaction.options.getString('symbol', true).toUpperCase();
  const name = interaction.options.getString('name', true);

  await interaction.reply({ content: '🚀 Launching your token...', ephemeral: true });

  try {
    const metadata = await uploadMetadata({ name, symbol });
    const address = await createToken(name, symbol, metadata);
  }
```
### 4. Post in Target Channel

```ts
  const embed = new EmbedBuilder()
    .setTitle(`🚀 Token Launched: ${name}`)
    .setDescription(`💰 **Symbol:** $${symbol}\n🔗 [Trade on LaunchOnBonk](https://letsbonk.fun/token/${address})`)
    .setImage(metadata.image || 'https://launchonbonk.online/default-token-image.png')
    .setColor(0x00ffae)
    .setFooter({ text: 'LaunchOnBonk — Powered by Solana' });

  const channel = await client.channels.fetch(CHANNEL_ID);
  if (channel?.isTextBased()) {
    await channel.send({ embeds: [embed] });
  }

  await interaction.editReply({ content: `✅ Token ${name} ($${symbol}) launched successfully!` });

} catch (err) {
  console.error('Launch error:', err);
  await interaction.editReply('❌ Failed to launch token. Please try again.');
}

```

### 5. Start Bot + Register Slash Command

```ts
client.once('ready', () => {
  console.log(`🤖 Logged in as ${client.user?.tag}`);
});

client.login(DISCORD_BOT_TOKEN);
registerCommands();
```

---

## 🛠 Coming Soon

- API Scale  
  → Optimize the system to increase coin generation and enhance processing speed for a faster, more efficient user experience.

- Reward Claims  
  → Users will be able to claim rewards directly on our website.

- Wallet Integration  
  → Seamless connection with your wallet.

---

## 🤝 Contributing

We welcome contributions from the community!

- Open issues for bugs, improvements, or feature requests.  
- Fork the repo and submit pull requests.  
- Join our community — we’re building for the long term.

---

## 📬 Contact

For questions or support, connect with us on [Twitter]([https://twitter.com](https://x.com/Launch_on_bonk)).
