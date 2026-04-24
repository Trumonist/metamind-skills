---
name: image-gen
description: Generate images with AI (DALL-E 3) when user asks to draw, create, paint, or generate a picture/image. Triggers: "нарисуй", "нарисовать", "сгенерируй картинку", "создай изображение", "придумай картинку", "draw", "generate image", "create image", "paint a picture", "make an image".
---

# image-gen

Генерирует изображения через OpenAI DALL-E 3 и отправляет пользователю прямо в Telegram.

## When to use

- Пользователь просит нарисовать / сгенерировать / создать картинку или изображение
- Ключевые слова: нарисуй, сгенерируй, создай картинку, придумай иллюстрацию, draw, generate image, create picture, paint

## When NOT to use

- Пользователь просит отредактировать или описать уже существующее изображение — это другой скилл
- Пользователь просит сделать диаграмму / схему — лучше mermaid или ASCII

## Step-by-step instructions

### 1. Извлечь промпт

Вычлени описание желаемой картинки из сообщения пользователя.  
Если описание расплывчато (< 5 слов смысловых), задай **один** уточняющий вопрос:  
> «Уточни стиль или детали: реализм, мультяшный, минимализм? Что должно быть в кадре?»

Не задавай больше одного вопроса — лучше сделай разумное предположение о стиле.

### 2. Улучшить промпт (prompt enhancement)

Переведи промпт на английский (DALL-E лучше работает с английским).  
Добавь детали стиля если их нет: `high quality, detailed, professional`.

### 3. Вызвать DALL-E 3 API

```bash
PROMPT="<улучшенный промпт>"

RESPONSE=$(curl -sS -X POST https://api.openai.com/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d "{
    \"model\": \"dall-e-3\",
    \"prompt\": \"$PROMPT\",
    \"n\": 1,
    \"size\": \"1024x1024\",
    \"quality\": \"standard\"
  }")

IMAGE_URL=$(echo "$RESPONSE" | python3 -c "import sys,json; d=json.load(sys.stdin); print(d['data'][0]['url'])" 2>/dev/null)
REVISED_PROMPT=$(echo "$RESPONSE" | python3 -c "import sys,json; d=json.load(sys.stdin); print(d['data'][0].get('revised_prompt',''))" 2>/dev/null)
```

Если `IMAGE_URL` пустой — распечатай сырой `$RESPONSE` для диагностики и сообщи пользователю об ошибке.

### 4. Скачать изображение

```bash
curl -sS -L "$IMAGE_URL" -o /tmp/generated_image.png
```

### 5. Отправить пользователю

```bash
curl -sS -F file=@/tmp/generated_image.png \
     -F token=$UPLOAD_TOKEN \
     $UPLOAD_URL
```

### 6. Ответить текстом

После отправки файла напиши короткое сообщение:

```
🎨 Готово! [краткое описание что нарисовано]

Хочешь изменить стиль, цвета или детали — скажи, сделаю новый вариант.
```

Если DALL-E изменил промпт (revised_prompt ≠ original), упомяни это одной строкой:
```
(DALL-E скорректировал запрос для лучшего результата)
```

## Error handling

| Ситуация | Действие |
|---|---|
| `OPENAI_API_KEY` не задан / 401 | «Ключ OpenAI API не настроен. Попроси администратора добавить OPENAI_API_KEY в окружение бота.» |
| 400 content policy | «DALL-E отклонил запрос по правилам контента. Попробуй переформулировать.» |
| 429 rate limit | «Слишком много запросов, подожди минуту и повтори.» |
| Файл не скачался (размер 0) | Повтори curl ещё раз; если снова 0 — сообщи пользователю |

## Notes

- Размер по умолчанию: `1024x1024`. Если пользователь просит широкоформатную — используй `1792x1024`; вертикальную — `1024x1792`.
- Модель: `dall-e-3` (не менять на dall-e-2, качество хуже).
- Не сохраняй сгенерированные изображения дольше сессии — они всё равно ephemeral.
