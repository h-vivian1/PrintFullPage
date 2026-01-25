# 🚀 Guia de Deploy - PrintFullPage

Este guia cobre o deploy "Híbrido": Backend no Render (para suportar o Puppeteer/Chrome) e Frontend na Vercel (para velocidade global).

---

## 🏗️ Parte 1: Backend (Render.com)

O Backend precisa de um ambiente Docker para rodar o Chrome.

1.  **Crie uma conta** no [Render.com](https://render.com/).
2.  Clique em **"New + "** -> **"Web Service"**.
3.  Conecte seu repositório do GitHub.
4.  Nas configurações do serviço:
    *   **Name:** `printfullpage-api` (ou o que preferir)
    *   **Language:** Selecione `Docker` (Isso é importante!).
    *   **Root Directory:** Digite `server` (pois o Dockerfile está dentro da pasta `server`).
    *   **Instance Type:** Free (ou Starter se quiser mais RAM).
5.  Clique em **"Create Web Service"**.
6.  Aguarde o deploy finalizar (pode demorar uns 5-10 minutos na primeira vez instalando o Chrome).
7.  **Copie a URL** gerada (ex: `https://printfullpage-api.onrender.com`). Você vai precisar dela na Parte 2.

> **Nota:** No plano gratuito do Render, o servidor entra em modo de espera após 15 minutos sem uso. O primeiro request pode demorar ~50 segundos para "acordar" o servidor.

---

## 🎨 Parte 2: Frontend (Vercel)

O Frontend é estático e super rápido na Vercel.

1.  **Crie uma conta** na [Vercel](https://vercel.com/).
2.  Clique em **"Add New..."** -> **"Project"**.
3.  Importe o mesmo repositório do GitHub.
4.  Na tela de configuração "Configure Project":
    *   **Root Directory:** Clique em "Edit" e selecione a pasta `client`.
    *   **Framework Preset:** Deve detectar `Vite` automaticamente.
    *   **Environment Variables:**
        *   Clique em para abrir a seção.
        *   **Key:** `VITE_API_URL`
        *   **Value:** Cole a URL do Backend (Sem a barra `/` no final). Ex: `https://printfullpage-api.onrender.com`
5.  Clique em **"Deploy"**.

---

## ✅ Testando

Acesse a URL que a Vercel gerou.
Tente gerar um print.
*   *Se o backend estiver dormindo (Render Free), vai ficar rodando o spinner por uns 50s na primeira vez. Isso é normal.*

---

## ⚠️ Limitações da Versão em Nuvem (Free Tier)

1.  **Persistência de Arquivos:**
    O sistema de arquivos do Render/Heroku/Railway é **efêmero**.
    *   Isso significa que os prints gerados ficam salvos temporariamente. Se o servidor reiniciar ou fizer um novo deploy, os prints antigos **somem**.
    *   Para o uso "ao vivo" (gerar e baixar na hora), funciona perfeitamente.
    *   *Solução pra escalar:* Integrar com AWS S3 para salvar os prints na nuvem de verdade.
