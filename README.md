        ref_code = context.args[0] если context.args иначе нет
        реферер = Нет

        если ref_code:
            conn = sqlite3.connect(ИМЯ_БД)
            курсор = conn.cursor()
            cursor.execute("ВЫБЕРИТЕ * ИЗ пользователей, ГДЕ referral_code=?", (ref_code,))
            реферер = курсор.fetchone()
            conn.закрыть()

        add_user(user_id, referral_code, referrer[0] если реферер, иначе None)
        reply_text = f"Ваш реферальный код: `{referral_code}`.\n\n" \
                     f"Ваша реферальная ссылка: `https://t.me/{context.bot.username}?start={referral_code}`"
        обновление.сообщение.ответ_текст(ответ_текст, parse_mode="Markdown")


def referrals(обновление, контекст):
    user_id = update.effective_user.id
    пользователь = получить_пользователя(user_id)

    если не пользователь:
        update.message.reply_text("Вы не зарегистрированы. Используйте /start для регистрации.")
        возвращаться

    referrals_list = get_referrals(пользователь[0])
    если referrals_list:
        referrals_count = len(referrals_list)
        сообщение = f"Ваши рефералы: {referrals_count}\n\n"
        для ссылки в referrals_list:
             сообщение += f"ID: {ref[1]}\n"
        обновление.сообщения.ответ_текст(сообщение)
    еще:
        update.message.reply_text("У вас пока нет рефералов.")


def help(обновление, контекст):
  update.message.reply_text("Команды:\n/start - запустить бота\n/referrals - просмотреть ваших рефералов\n/help - показать это сообщение")


определение main():
  создать_базу_данных()
  обновитель = Обновитель(ТОКЕН, use_context=True)
  dp = обновитель.диспетчер

  dp.add_handler(CommandHandler("старт", старт))
  dp.add_handler(CommandHandler("рефералы", рефералы))
  dp.add_handler(CommandHandler("помощь", помощь))

  updater.start_polling()
  logger.info("Бот запущен")
  updater.idle()


если __name__ == '__main__':
    основной()
