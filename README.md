# Eternal_bot
import logging
from aiogram import Bot, Dispatcher, types
from aiogram.contrib.middlewares.logging import LoggingMiddleware
from aiogram.types import ReplyKeyboardMarkup, KeyboardButton
import random

TOKEN = "7530789858:AAF6KB25JpPvpu_CZwVLuHrY_DjxwFL-Xik"

bot = Bot(token=TOKEN)
dp = Dispatcher(bot)
dp.middleware.setup(LoggingMiddleware())

keyboard = ReplyKeyboardMarkup(resize_keyboard=True)
keyboard.add(KeyboardButton("🚀 Сигнали"))
keyboard.add(KeyboardButton("📊 Статистика"))

games = {
    "Mines": ["💣💣💣💎💎", "💎💎💣💣💣", "💣💎💣💎💎"],
    "Lucky Jet": ["1.5x 🚀", "2x 🚀", "3x 🚀", "CRASH 💥"],
    "Towers": ["⬆️⬆️⬇️⬆️⬇️", "⬇️⬆️⬇️⬆️⬆️"],
}

@dp.message_handler(commands=["start"])
async def start(message: types.Message):
    await message.answer("👋 Ласкаво просимо! Натисніть кнопку, щоб отримати сигнали!", reply_markup=keyboard)

@dp.message_handler(lambda message: message.text == "🚀 Сигнали")
async def send_signal(message: types.Message):
    game = random.choice(list(games.keys()))
    signal = random.choice(games[game])
    await message.answer(f"🎯 Гра: {game}\n📌 Сигнал: {signal}")

if __name__ == "__main__":
    from aiogram import executor
    executor.start_polling(dp, skip_updates=True)
