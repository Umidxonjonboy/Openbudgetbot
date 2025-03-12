import logging
from aiogram import Bot, Dispatcher, types
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton
from aiogram.utils import executor

# 🔹 Sizning ma'lumotlaringiz
TOKEN = "8041416221:AAE5iOK1z7CbKPRpDEYfPNyOiKkMF27RgoA"  # Bot token
ADMIN_ID = 6348366872  # Admin ID
VOTE_LINK = "https://openbudget.uz/boards/initiatives/initiative/50/fa2a53a7-fb29-4a1f-bd93-9618ebf3fdc8"  # Ovoz berish havolasi

# 🔹 Logger sozlash
logging.basicConfig(level=logging.INFO)

# 🔹 Bot va Dispatcher yaratish
bot = Bot(token=TOKEN)
dp = Dispatcher(bot)

# 🔹 Start komandasi
@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    keyboard = InlineKeyboardMarkup()
    keyboard.add(InlineKeyboardButton("✅ Ovoz berish", url=VOTE_LINK))
    keyboard.add(InlineKeyboardButton("📤 Ovoz berdim", callback_data="verify_vote"))
    
    await message.answer("👋 Assalomu alaykum!\n📌 Open Budget loyihasiga ovoz bering va tasdiqlang!", reply_markup=keyboard)

# 🔹 "Ovoz berdim" tugmasi bosilganda
@dp.callback_query_handler(lambda call: call.data == "verify_vote")
async def verify_vote(call: types.CallbackQuery):
    keyboard = InlineKeyboardMarkup()
    keyboard.add(InlineKeyboardButton("📩 Admin bilan bog‘lanish", url=f"tg://user?id={ADMIN_ID}"))

    await call.message.answer("📸 Ovoz berganingizni tasdiqlash uchun admin bilan bog‘laning va skrenshot yuboring.", reply_markup=keyboard)

# 🔹 Xatolarni qayta ishlash
@dp.errors_handler()
async def error_handler(update, exception):
    logging.error(f"Xatolik: {exception}")
    return True

# 🔹 Botni ishga tushirish
if name == "__main__":
    executor.start_polling(dp, skip_updates=True)
