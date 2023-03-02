from asyncio.windows_events import NULL
from os import getenv, name
from tracemalloc import stop
from urllib import request

import random
import discord
import requests
from discord import Intents, Message
from discord.ext import commands
from discord.ext.commands import Context
from dotenv import load_dotenv
from notifiers import get_notifier

hangmani = []
slovnik = {}

load_dotenv()
TOKEN = getenv("DISCORD_TOKEN")
IMGFLIP_API_URL = "https://api.imgflip.com"

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix="!",
                   case_insensitive=True, intents=intents)


class MemeGenerator:
    def __init__(self) -> None:
        # Sem môžete pridať vlastné atribúty.
        pass

    def list_memes(self) -> str:
        #Pošle do chatu 25 nejpopulárnějších meme
        vysledek = ""
        meme_id = []
        meme_name = []
        response = requests.get("https://api.imgflip.com/get_memes")
        text = response.json()
        alternated_text = text["data"]["memes"]
        for i in range(25):
            meme_id = alternated_text[i]["id"]
            meme_name = alternated_text[i]["name"]
            vysledek += meme_id + " " + meme_name + "\n"
        return vysledek

    def make_meme(self, template_id: int,
                  top_text: str, bottom_text: str) -> str:
        #umožní uřivateli upravit meme

        # Vráťte URL vygenerovaného meme.
        parameters = {
            "template_id": template_id,
            "username": uživatelské jméno,
            "password": uživatelské heslo,
            "text0": top_text,
            "text1": bottom_text,
        }
        response = requests.post(
            "https://api.imgflip.com/caption_image", data=parameters
        )
        a = response.json()
        if a["success"] is not True:
            return "Invalid template"
        return a["data"]["url"]


class MentionsNotifier:
    def __init__(self) -> None:
        self.__emaily = {}
        pass

    def subscribe(self, user_id: int, email: str) -> None:
        if user_id in self.__emaily:
            self.__emaily[user_id] = email
        else:
            self.__emaily[user_id] = email
        pass

    def unsubscribe(self, user_id: int) -> None:
        if user_id in self.__emaily:
            del self.__emaily[user_id]
        else:
            pass
        pass

    def notify_about_mention(self, user_id: int, msg_content: str) -> None:
        if user_id not in self.__emaily:
            pass
        else:
            email = get_notifier("email")
            telo_email = "Someone mentioned you in channel " + msg_content
            settings = {
                "host": hostovský email,
                "port": 465,
                "ssl": True,
                "username": přihlašovací jméno,
                "password": přihlašovací heslo,
                "to": self.__emaily[user_id],
                "from": název účtu,
                "subject": "Mentioned in discord",
                "message": telo_email,
            }

            res = email.notify(**settings)

            pass


class Hangman:
    #Vytvoření hry HANGMAN
    def __init__(self) -> None:
        # Sem môžete pridať vlastné atribúty.
        self.hledane_slovo = []
        self.uhadnute_slovo = []
        self.hadana_pismena = []
        self.zivoty = 7
        self.id: int
        self.jmeno: str
        pass

    def create_hangman(self, user_name: str):
        vysledek = ""
        slovo = ""
        self.hledane_slovo = []
        self.uhadnute_slovo = []
        self.hadana_pismena = []
        self.zivoty = 7
        self.id: int
        self.jmeno = user_name

        with open("words.txt", encoding="utf-8", mode="r") as f:
            a = f.readlines()
            for i in range(len(a)):
                a[i] = a[i].strip()
        b = random.choice(a)
        for i in b:
            self.hledane_slovo.append(i)
            self.uhadnute_slovo.append("-")

        vysledek = (
            "**Hangman** \nPlayer: "
            + user_name
            + "\nGuesses: "
            + "\nLives: "
            + str(self.zivoty)
            + "\nWord: "
            + " ".join(self.uhadnute_slovo)
        )
        return vysledek

    def hg_guess(self, letter: str, user_name: str):
        letter = letter.lower()
        pokus = 1
        slovo = ""
        dalsi_slovo = ""
        if letter in self.hadana_pismena:
            vysledek = (
                "**Hangman** \nPlayer: "
                + user_name
                + "\nGuesses: "
                + (" ".join(self.hadana_pismena))
                + "\nLives: "
                + str(self.zivoty)
                + "\nWord: "
                + " ".join(self.uhadnute_slovo)
                + "\nYou already guessed that."
            )
            return vysledek
        self.hadana_pismena.append(letter)
        if letter in self.hledane_slovo:
            for i in range(len(self.hledane_slovo)):
                if self.hledane_slovo[i] == letter:
                    self.uhadnute_slovo[i] = letter
            for i in range(len(self.hledane_slovo)):
                if self.uhadnute_slovo[i] == self.hledane_slovo[i]:
                    pokus = pokus * 1
                else:
                    pokus = pokus * 0
            if pokus == 1:
                vysledek = (
                    "**Hangman** \nPlayer: "
                    + user_name
                    + "\nGuesses: "
                    + (" ".join(self.hadana_pismena))
                    + "\nLives: "
                    + str(self.zivoty)
                    + "\nWord: "
                    + " ".join(self.uhadnute_slovo)
                    + "\nYou won!"
                )
                return vysledek
            elif pokus == 0:
                vysledek = (
                    "**Hangman** \nPlayer: "
                    + user_name
                    + "\nGuesses: "
                    + (" ".join(self.hadana_pismena))
                    + "\nLives: "
                    + str(self.zivoty)
                    + "\nWord: "
                    + (" ".join(self.uhadnute_slovo))
                    + "\nCorrect guess."
                )
                return vysledek

        else:
            self.zivoty -= 1
            if self.zivoty < 1:
                vysledek = (
                    "**Hangman** \nPlayer: "
                    + user_name
                    + "\nGuesses: "
                    + (" ".join(self.hadana_pismena))
                    + "\nLives: "
                    + str(self.zivoty)
                    + "\nWord: "
                    + " ".join(self.uhadnute_slovo)
                    + "\nYou lost! The word was: "
                    + "".join(self.hledane_slovo)
                    + "."
                )
                return vysledek
            else:
                vysledek = (
                    "**Hangman** \nPlayer: "
                    + user_name
                    + "\nGuesses: "
                    + (" ".join(self.hadana_pismena))
                    + "\nLives: "
                    + str(self.zivoty)
                    + "\nWord: "
                    + (" ".join(self.uhadnute_slovo))
                    + "\nWrong guess."
                )
                return vysledek


# --------- LEVEL 1 ----------
meme_generator = MemeGenerator()


@bot.command(name="list_memes")
async def list_memes(ctx: Context) -> None:
    meme_list = meme_generator.list_memes()
    # TODO: Poslať meme_list do kanála.
    await ctx.send(meme_list)


@bot.command(name="make_meme")
async def make_meme(
    ctx: Context, template_id: int, top_text: str, bottom_text: str
) -> None:
    meme_url = meme_generator.make_meme(template_id, top_text, bottom_text)
    # TODO: Poslať meme_url do kanála.
    await ctx.send(meme_url)


# --------- LEVEL 2 ----------
mentions_notifier = MentionsNotifier()


@bot.command(name="subscribe")
async def subscribe(ctx: Context, email: str) -> None:
    mentions_notifier.subscribe(ctx.author.id, email)


@bot.command(name="unsubscribe")
async def unsubscribe(ctx: Context) -> None:
    mentions_notifier.unsubscribe(ctx.author.id)


@bot.event
async def on_message(message: Message) -> None:
    #Při označení pošle upozornění
    promena_a = message.mentions
    if promena_a != []:
        for i in range(len(promena_a)):
            b = promena_a[i].id
            c = message.jump_url
            mentions_notifier.notify_about_mention(b, c)
            pass
    else:
        pass
    # Nasledujúci riadok nemodifikujte
    # , inak príkazy bota nebudú fungovať.
    await bot.process_commands(message)


# --------- LEVEL 3 ----------
hangman = Hangman()


@bot.command(name="play_hangman")
async def play_hangman(ctx: Context) -> None:
    # TODO: Implementujte tento príkaz s využitím triedy Hangman.
    slovnik[ctx.author.name] = len(slovnik)
    hangman = Hangman()
    hangmani.append(hangman)
    a = await ctx.send(
        hangmani[slovnik[ctx.author.name]].create_hangman(ctx.author.name)
    )
    hangmani[slovnik[ctx.author.name]].id = a.id
    pass


@bot.command(name="guess")
async def guess(ctx: Context, letter: str) -> None:
    # TODO: Implementujte tento príkaz s využitím triedy Hangman.
    if len(letter) == 1:
        if ctx.author.name in slovnik:
            if hangmani[slovnik[ctx.author.name]].id is not None:
                await ctx.message.delete()
                p = slovnik[ctx.author.name]
                msg = await ctx.fetch_message(hangmani[p].id)
                c = hangmani[slovnik[ctx.author.name]]
                pom = letter
                n = ctx.author.name
                await msg.edit(content=c.hg_guess(pom, n))
                if hangmani[slovnik[ctx.author.name]].zivoty < 1:
                    hangmani[slovnik[ctx.author.name]].id = None
                pass
        else:
            await ctx.send("Zapnete hangmana!")
            pass
    else:
        await ctx.send("Pouze jedno pismeno!")
        pass


bot.run(TOKEN)
