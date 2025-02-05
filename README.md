import os
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.messages = True
intents.message_content = True

bot = commands.Bot(command_prefix="!", intents=intents)

# Elixir-Werte und ihre Multiplikatoren
elixir_values = {
    30: 1.2,
    100: 1.4,
    200: 1.7,
    350: 2.0,
    600: 2.3,
    1000: 2.6,
    1600: 2.9,
    2300: 3.3,
    3150: 3.7,
    4000: 4.1,
    4850: 4.5,
    5700: 5.0,
    6550: 5.5,
    7400: 6.0,
    8250: 6.5,
    9100: 7.1,
    10000: 7.7,
    11000: 8.3,
    13000: 8.9,
    15000: 9.6,
    17000: 10.3,
    19000: 11.0,
    22000: 11.8,
    25000: 12.6,
    30000: 13.4,
    37000: 14.3,
    46000: 15.2,
    57000: 16.1,
    70000: 17.0,
    85000: 18.0,
    105000: 19.0,
    130000: 20.0,
    160000: 21.0,
}

@bot.command()
async def troops(ctx, troop_value: int, specific_elixir: int = None):
    # Zuerst die Nachricht "VALAR MORGHULIS" senden
    await ctx.send("**VALAR MORGHULIS**")

    if specific_elixir:
        # Berechnung nur für den spezifischen Elixierwert
        if specific_elixir in elixir_values:
            multiplier = elixir_values[specific_elixir]
            result = int(troop_value * multiplier)  # Umwandlung in Ganzzahl
            await ctx.send(
                f"For elixir {specific_elixir}, new troop value is: {result}."
            )
        else:
            await ctx.send(f"Elixir value {specific_elixir} not found in predefined values.")
    else:
        # Berechnung für alle Elixierwerte
        response = f"**Troops calculation - All combos**\nGiven troop value: {troop_value}\n\n"
        for elixir, multiplier in elixir_values.items():
            result = int(troop_value * multiplier)  # Umwandlung in Ganzzahl
            response += (f"For elixir {elixir}, new troop value: {result}\n")
            if len(response) > 1900:  # Nachricht unterteilen, wenn zu lang
                await ctx.send(response)
                response = ""
        if response.strip():
            await ctx.send(response)

from flask import Flask
from threading import Thread

app = Flask('')

@app.route('/')
def home():
    return "Bot is running!"

def run():
    app.run(host='0.0.0.0', port=8080)

def keep_alive():
    t = Thread(target=run)
    t.start()

DISCORD_TOKEN = os.environ['DISCORD_TOKEN']
bot.run(DISCORD_TOKEN)
    
