![Launch On Discord Banner](./githubbanner.jpg)

# BONKCORD 

Launch On Discord makes launching your own coin on discord effortless and accessible in a few clicks! 

🔗 [Check out our website → launchonbonk.online](https://bonkcord.fun/)

---

## Features w Bonkcord

- No Fees  
  → ZERO trading fees

- Built on Solana  
  → Fast transactions, low fees, and high scalability.

- Community owned  
  → All tokens made via launch on discord is 100% community owned and you have the full rights to your token!

- Fast  
  → We made the tool work as fast as possible, test yourself :)

- Rewards  
  → Releasing a reward system that incentivizes coin longevity

## How does it work? - run through our code to see how Bonkcord works!

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

## Coming Soon

- Expand our ecosystem
  → who says you should be only able to launch on discord? we plan to expand our ecosystem with so users can launch tokens on mutiple social platforms such as TikTok, Reddit, Wechat and more, all by the same token! 

- Reward Claims  
  → Users will be able to claim rewards directly on our website.

- Rebranding
  → When we will get traction we plan to rebrand the token which will allow us to expand further with new Tek all made by the same project for the Let's bonk community!
---

## Contributing

We welcome contributions from the community!

- Open issues for bugs, improvements, or feature requests.  
- Fork the repo and submit pull requests.  
- Join our community — we’re building for the long term.

---

## Contact

For questions or support, connect with us on [Twitter]([https://x.com/bonk_cord)).
