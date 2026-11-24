# Guia de GeraÃ§Ã£o de VÃ­deo (SkyReels V3)

Este arquivo define as regras para geraÃ§Ã£o de vÃ­deos via SkyReels V3.
O campo `prompt` de cada cena usa estas diretrizes.

## Tasks disponÃ­veis
- **reference_to_video**: gera vÃ­deo a partir de 1â€“4 imagens de referÃªncia + prompt
- **single_shot_extension**: estende vÃ­deo existente (5â€“30s)
- **shot_switching_extension**: estende com transiÃ§Ã£o cinemÃ¡tica (mÃ¡x. 5s)
- **talking_avatar**: avatar falante (retrato + Ã¡udio, atÃ© 200s)

## Regras do prompt de vÃ­deo
- Escreva em INGLÃŠS
- Inclua: composiÃ§Ã£o, iluminaÃ§Ã£o, movimento de cÃ¢mera, emoÃ§Ã£o, aÃ§Ã£o dos personagens
- Seja especÃ­fico sobre direÃ§Ãµes (up/down/left/right) â€” o vÃ­deo seguirÃ¡ exatamente
- âš  COERÃŠNCIA: o prompt de vÃ­deo DEVE descrever a mesma aÃ§Ã£o/direÃ§Ã£o que o audio_text

## ReferÃªncias visuais (ref_imgs)
- MÃXIMO 4 imagens por cena
- Use SEMPRE a imagem do ambiente + imagem do personagem que aparece
- NUNCA duas imagens do mesmo personagem (duplica o personagem na cena)

## Exemplo de prompt
"Medium shot, Valen stands at school corridor, morning sunlight through windows,
she turns to look at Lumi, curious expression, soft camera pan right,
anime style, 2030 futuristic school, cARCKHÉtic lighting"
