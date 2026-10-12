---
name: "seedance-3d-cgi"
description: "Escreve prompts de vídeo 3D/CGI para Seedance no Higgsfield, com foco em animações didáticas de anatomia e fisiopatologia para aulas (MedCof). Use para 3D, CGI, render, animação de anatomia."
---

# Seedance 3D CGI para aula médica

Base: skill 02-3d-cgi do repositório AKCodez/higgsfield-claude-skills (framework de hook, template e checklists), condensada e adaptada para conteúdo médico didático.

## Fluxo

1. Ler o slide da aula que a animação vai substituir. A animação segue a ordem didática do slide.
2. Gerar primeiro um quadro-âncora (imagem) com gpt_image_2_5, count 4, 16:9. Custo baixo. Escolher o anatomicamente mais correto olhando as 4 lado a lado.
3. Gerar o vídeo com seedance_2_5, mode omni_reference, a imagem escolhida como start_image, generate_audio false.
4. Sempre fazer draft 480p primeiro (cerca de 1/4 do custo). Revisar. Só então finalizar em 1080p com draft_job_id.
5. Rótulos entram depois, no PowerPoint ou no editor. Nunca pedir texto ao modelo de vídeo: ele deforma letras e erra termos.

## Regras de anatomia no prompt

- Descrever cada estrutura com material próprio para que se separem visualmente (cor, brilho, textura).
- Escrever explicitamente as relações que o modelo costuma errar. Exemplo: a sinóvia reveste a face interna da cápsula e termina na margem da cartilagem; não cobre a cartilagem.
- Travar a anatomia: mesma vista, osso parado, sem rotação, sem deformação, plano contínuo sem cortes.
- Proibir texto: no text, labels, letters, numbers, arrows, logos, watermark.

## Técnica de destaque sequencial

Para explicar N estruturas em um plano só: em cada beat uma estrutura brilha (emissive rim light) e as demais escurecem para ~40%. Uma cor por estrutura. Último beat: tudo aceso, volta ao enquadramento inicial e segura parado 1 s para o professor rotular.

## Template de prompt

OPENING HOOK: 1 a 2 frases com o impacto visual dos primeiros 2 s.
Render style: photorealistic scientific medical 3D animation, PBR materials.
Regras de consistência: mesma vista, sem cortes, sem rotação.
Timeline: beats com segundos exatos (0-3s, 3-9s...). Cada beat diz o que acende, o que se move e o que a câmera faz.
Camera: tipo de movimento, ease-in/ease-out, 24fps.
Lighting: key 5600K upper left, cool fill, rim light, fundo navy-to-black.
Negativas: sem texto, sem rótulos.

## Vocabulário útil

Hooks: push-in lento revelando estrutura; revelação por transparência (camada externa fica translúcida); scale reveal (da articulação ao capilar); partículas que se organizam em estrutura.
Câmera: slow push-in, glide along interface, ease back, static hold with internal motion, orbit lento (evitar em anatomia de corte).
Materiais: hyaline cartilage com subsurface scattering azul-perolado; trabecular bone honeycomb com medula amarela; synovium velvety pink-red com capilares; fibrous capsule com estrias de colágeno branco-prata; fluido claro viscoso.
Luz: 3-point studio; rim light para separar do fundo; Kelvin explícito.

## Erros comuns

- Câmera vaga ("dinâmica"). Especificar tipo, velocidade e segundos.
- Duração não especificada. Sempre dar os segundos de cada beat.
- Cortes múltiplos. O modelo prefere plano contínuo.
- Prompt longo demais para clipe curto. Para 5 s, 8 linhas bastam.
- Pedir patologia e anatomia normal no mesmo clipe. Separar em dois vídeos: normal e doente (ex.: sinovite e pannus).

## Checklist antes de gerar

Hook no início. Beats com segundos. Câmera com easing. Cor por estrutura. Relação anatômica crítica escrita. Proibição de texto. Draft antes do 1080p. Custo conferido com get_cost.