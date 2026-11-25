# Guia de GeraêÂ§êÂ£o de VêÂ­deo (SkyReels V3)

Este arquivo define as regras para geraêÂ§êÂ£o de vêÂ­deos via SkyReels V3.
O campo `prompt` de cada cena usa estas diretrizes.

## Tasks disponêÂ­veis
- **reference_to_video**: gera vêÂ­deo a partir de 1ê¢â¬â4 imagens de referêÂªncia + prompt
- **single_shot_extension**: estende vêÂ­deo existente (5ê¢â¬â30s)
- **shot_switching_extension**: estende com transiêÂ§êÂ£o cinemêÂ¡tica (mêÂ¡x. 5s)
- **talking_avatar**: avatar falante (retrato + êÂ¡udio, atêÂ© 200s)

## Regras do prompt de vêÂ­deo
- Escreva em INGLêÅ S
- Inclua: composiêÂ§êÂ£o, iluminaêÂ§êÂ£o, movimento de cêÂ¢mera, emoêÂ§êÂ£o, aêÂ§êÂ£o dos personagens
- Seja especêÂ­fico sobre direêÂ§êÂµes (up/down/left/right) ê¢â¬â o vêÂ­deo seguirêÂ¡ exatamente
- ê¢Å¡Â  COERêÅ NCIA: o prompt de vêÂ­deo DEVE descrever a mesma aêÂ§êÂ£o/direêÂ§êÂ£o que o audio_text

## ReferêÂªncias visuais (ref_imgs)
- MêÂXIMO 4 imagens por cena
- Use SEMPRE a imagem do ambiente + imagem do personagem que aparece
- NUNCA duas imagens do mesmo personagem (duplica o personagem na cena)

## Exemplo de prompt
"Medium shot, Valen stands at school corridor, morning sunlight through windows,
she turns to look at Lumi, curious expression, soft camera pan right,
anime style, 2030 futuristic school, cARCKHêtic lighting"
