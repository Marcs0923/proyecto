import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Hola, soy un bot {bot.user}!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("he" * count_heh)

@bot.command()
async def bye(ctx):
    await ctx.send(f'Adios')

@bot.command()
async def suma(ctx,num1:int,num2:int):
    await ctx.send(f"la suma de {num1} y {num2} es {num1+num2}")

@bot.command()
async def resta(ctx,num1:int,num2:int):
    await ctx.send(f"la resta de {num1} y {num2} es {num1-num2}")

@bot.command()
async def poke(ctx,arg):
    try:
        pokemon = arg.split(" ",1)[0].lower()
        result = requests.get("https://pokeapi.co/api/v2/pokemon/"+pokemon)
        if result.text == "Not Found":
            await ctx.send("Pokemon no encontrado")
        else:
            image_url = result.json()["sprites"]["front_default"]
            print(image_url)
            await ctx.send(image_url)
    except Exception as e:
        print("Error:", e)
@poke.error
async def error_type(ctx,error):
    if isinstance(error,commands.errors.MissingRequiredArgument):
        await ctx.send("Tienes que darme un pokemon")

@bot.command()
async def limpiar(ctx):
    await ctx.channel.purge()
    await ctx.send("Mensajes eliminados", delete_after = 3)

@bot.command()
async def alimentacion(ctx):
    await ctx.send(f"""Hola, soy un bot {bot.user}!""")# esta linea saluda
    await ctx.send(f'Te voy hablar un poco sobre la alimentacion')
    await ctx.send(f'La buena alimentacion es una parte crucial en el dia a dia del ser humano para mantenerse sano')
    # Enviar una pregunta al usuario
    await ctx.send("Quieres consejos sobre cómo mantener una buena alimentacion? Responde con 'sí' o 'no'.")
# Esperar la respuesta del usuario
    def check(message):
        return message.author == ctx.author and message.channel == ctx.channel and message.content in ['sí', 'si', 'no']
    response = await bot.wait_for('message', check=check)
    if response:
        if response.content in ['sí', 'si']:
            await ctx.send("1. No comer mucha azucar")
            await ctx.send("2. No saltarse ninguna comida del dia ")   
        else:
            await ctx.send("Está bien, si alguna vez necesitas consejos, no dudes en preguntar.")
    else:
        await ctx.send("Lo siento, no pude entender tu respuesta. Inténtalo de nuevo.")
    await ctx.send("Quieres saber la definicion de alimentacion, responde si o no")
    response1 = await bot.wait_for('message', check=check)
    if response1:
        if response1.content in ['sí', 'si']:
            await ctx.send("La alimentación es el proceso de elegir, preparar y consumir alimentos para obtener energía y sustancias nutritivas") 
        else:
            await ctx.send("Está bien, si alguna vez necesitas consejos, no dudes en preguntar.")
    else:
        await ctx.send("Lo siento, no pude entender tu respuesta. Inténtalo de nuevo.")

def get_perros_image_url():    
    url = 'https://random.dog/woof.json'
    res = requests.get(url)
    data = res.json()
    return data['url']

@bot.command('perros')
async def perros(ctx):
    '''Una vez que llamamos al comando duck, 
    el programa llama a la función get_duck_image_url'''
    image_url = get_perros_image_url()
    await ctx.send(image_url)

bot.run("TOKEN")
