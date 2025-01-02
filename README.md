const { Client, GatewayIntentBits } = require('discord.js');
const client = new Client({ intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent] });

// Substitua pelo seu token do bot
const token = 'SEU_TOKEN_AQUI';

client.once('ready', () => {
    console.log('Bot está online!');
});

client.on('messageCreate', async (message) => {
    // Comando para criar canais de compra e suporte
    if (message.content === '!criar-canais') {
        if (message.member.permissions.has('ADMINISTRATOR')) {
            // Criar canais
            let compraChannel = await message.guild.channels.create('🛒 | Compra', {
                type: 'GUILD_TEXT',
                topic: 'Canal para compras de produtos!',
                permissionOverwrites: [
                    {
                        id: message.guild.id,
                        deny: ['SEND_MESSAGES'],
                    },
                ],
            });

            let suporteChannel = await message.guild.channels.create('💬 | Suporte', {
                type: 'GUILD_TEXT',
                topic: 'Canal de suporte para dúvidas!',
                permissionOverwrites: [
                    {
                        id: message.guild.id,
                        deny: ['SEND_MESSAGES'],
                    },
                ],
            });

            // Enviar mensagem de aviso para os canais
            compraChannel.send('⚡ **Clique abaixo para comprar!** ⚡\n**Comando para iniciar a compra: `!comprar`**');
            suporteChannel.send('🔧 **Clique abaixo para abrir um ticket de suporte!** 🔧\n**Comando para abrir ticket: `!suporte`**');

            message.reply('Canais criados com sucesso!');
        } else {
            message.reply('Você não tem permissão para criar canais!');
        }
    }

    // Comando de compra
    if (message.content === '!comprar') {
        let comprarMessage = '🔶 **Compra de produtos** 🔶\nClique para confirmar a compra! **Descreva o produto que deseja.**';
        message.channel.send(comprarMessage);
    }

    // Comando de suporte
    if (message.content === '!suporte') {
        let suporteMessage = '🔧 **Abrindo ticket de suporte!** 🔧\nPor favor, explique seu problema ou dúvida e um de nossos gerentes irá te ajudar.';
        message.channel.send(suporteMessage);
    }
});

// Logando o bot
client.login(token);
