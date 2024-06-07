## Descripción del Código
Este script Python implementa un bot de Telegram que puede realizar operaciones básicas de suma, resta, multiplicación y división. 
- Cuando un usuario envía el comando `/start`, el bot le da la bienvenida y le proporciona instrucciones sobre cómo utilizarlo. 
- Al enviar el comando `/help`, el bot responde con información sobre cómo realizar operaciones matemáticas. 
- Además, el bot puede calcular el resultado de una expresión matemática que un usuario envíe como mensaje de texto y responder con el resultado. 
- El bot utiliza la función `eval()` para evaluar la expresión matemática ingresada por el usuario.

## Descripción del Código

![Evidencia](https://github.com/Marr1731/ActividadesLenguajesAutonomas/blob/main/img/evidencia.png)


## Código
```python
import logging
import re
import random

from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Envía un mensaje de bienvenida cuando se emite el comando /start."""
    user = update.effective_user
    await update.message.reply_html(
        rf"¡Hola {user.first_name}! Soy un bot que puede realizar operaciones básicas de suma, resta, multiplicación y división. Envíame un mensaje con la operación que quieres realizar, por ejemplo: '2 + 2' o '/help' para obtener ayuda."
    )


async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Envía un mensaje con información de ayuda cuando se emite el comando /help."""
    await update.message.reply_text("Puedes realizar operaciones básicas de suma, resta, multiplicación y división. Por ejemplo: '2 + 2', '5 * 3', '10 / 2', etc.")


async def calculate(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Realiza la operación matemática especificada por el usuario."""
    message_text = update.message.text.strip()
    
    try:
        # Evaluamos la expresión matemática utilizando la función eval()
        result = eval(message_text)
        response = f"El resultado es: {result}"
    except Exception as e:
        response = "Ha ocurrido un error al calcular. Por favor, verifica la expresión e inténtalo de nuevo."

    await context.bot.send_message(chat_id=update.effective_chat.id, text=response)


def main() -> None:
    """Inicio del bot."""
    application = Application.builder().token("TU_TOKEN_DE_BOT").build()  # Reemplaza "TU_TOKEN_DE_BOT" con tu token de bot de Telegram

    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("help", help_command))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, calculate))

    application.run_polling(allowed_updates=Update.ALL_TYPES)


if __name__ == "__main__":
    main()
