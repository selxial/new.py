import discord
import random


intents = discord.Intents.default()

intents.message_content = True

client = discord.Client(intents=intents)


trivia_questions = [
    {
        "question": "What is the capital of France?",
        "options": ["A. Paris", "B. London", "C. Berlin", "D. Madrid"],
        "answer": "A"
    },
    {
        "question": "Who wrote 'To Kill a Mockingbird'?",
        "options": ["A. Harper Lee", "B. Mark Twain", "C. J.K. Rowling", "D. Ernest Hemingway"],
        "answer": "A"
    },
    {
        "question": "What is the smallest planet in our solar system?",
        "options": ["A. Earth", "B. Mars", "C. Mercury", "D. Venus"],
        "answer": "C"
    }
]

@client.event
async def on_ready():
    print(f'We have logged in as {client.user}')

@client.event
async def on_message(message):
    if message.author == client.user:
        return
    
    if message.content.startswith('$halo'):
        await message.channel.send("Hi!")
    elif message.content.startswith('$bye'):
        await message.channel.send("\\U0001f642")
    elif message.content.startswith('$trivia'):
        question = random.choice(trivia_questions)
        options = "\n".join(question["options"])
        await message.channel.send(f"Trivia Time! {question['question']}\n{options}")
        
        def check(m):
            return m.author == message.author and m.channel == message.channel
        
        try:
            response = await client.wait_for('message', check=check, timeout=30.0)
            if response.content.upper() == question["answer"]:
                await message.channel.send("Correct! 🎉")
            else:
                await message.channel.send(f"Wrong! The correct answer was {question['answer']}.")
        except asyncio.TimeoutError:
            await message.channel.send("You took too long to respond!")
    
    else:
        await message.channel.send(message.content)

client.run("bot token di sini")
