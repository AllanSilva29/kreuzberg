# 📄 Kreuzberg — Guia Prático (CLI + API com Docker)

Este guia mostra, de forma direta e operacional, como usar o **Kreuzberg** para extrair texto de PDFs (inclusive escaneados) e outros formatos.

---

# 🧠 Conceito rápido

O Kreuzberg:

* Extrai texto de arquivos (PDF, DOCX, imagens, etc.)
* Pode usar OCR quando o arquivo é imagem (PDF escaneado)
* Retorna JSON estruturado

---

# 🚀 1. Rodando via Docker (API)

## Subir o servidor

```bash
docker run -p 8001:8000 ghcr.io/kreuzberg-dev/kreuzberg:latest
```

A API ficará disponível em:

```
http://localhost:8001
```

---

# 🔍 2. Testar se está funcionando

```bash
curl http://localhost:8001/health
```

---

# 📄 3. Extração básica de PDF

```bash
curl -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf"
```

---

# 📄 4. Salvando o resultado

```bash
curl -s -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  > output.json
```

---

# 🧾 5. Extrair só o texto (sem JSON)

```bash
curl -s -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  | jq -r '.[0].content' > texto.txt
```

---

# 🧠 6. PDF escaneado (IMPORTANTE)

Se o output vier vazio → o PDF é imagem.

## Ativar OCR

```bash
curl -s -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  -F 'config={"ocr": {"enabled": true}}' \
  > output.json
```

---

# ⚙️ 7. OCR com mais controle

```bash
curl -s -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  -F 'config={
    "ocr": {
      "enabled": true,
      "backend": "tesseract"
    }
  }' \
  > output.json
```

---

# 📚 8. Outros tipos de arquivo

Funciona igual:

## DOCX

```bash
curl -F "files=@arquivo.docx" http://localhost:8001/extract
```

## Imagem (PNG/JPG)

```bash
curl -F "files=@imagem.png" http://localhost:8001/extract
```

## Excel

```bash
curl -F "files=@arquivo.xlsx" http://localhost:8001/extract
```

---

# 🔗 9. Múltiplos arquivos

```bash
curl -X POST http://localhost:8001/extract \
  -F "files=@file1.pdf" \
  -F "files=@file2.pdf"
```

---

# 🧩 10. Chunking (para LLM)

```bash
curl -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  -F 'config={
    "chunking": {
      "max_characters": 1000,
      "overlap": 100
    }
  }'
```

---

# 🧠 11. Pipeline completo (OCR + texto limpo)

```bash
curl -s -X POST http://localhost:8001/extract \
  -F "files=@arquivo.pdf" \
  -F 'config={"ocr": {"enabled": true}}' \
  | jq -r '.[0].content' > texto.txt
```

---

# 🔍 12. Ver formatos suportados

```bash
curl http://localhost:8001/formats
```

---

# 🧪 13. Debug útil

## Ver logs

```bash
docker logs <container_id>
```

## Listar containers

```bash
docker ps
```

---

# ⚠️ Problemas comuns

## ❌ "No files provided"

* Usou `file` ao invés de `files`

## ❌ Output vazio

* PDF escaneado → usar OCR

## ❌ Porta não abre

* Porta já em uso

---

# 💡 Dicas finais

* PDFs digitais → extração perfeita
* PDFs escaneados → OCR obrigatório
* Matemática → pode sair distorcida

---

# 🏁 Resultado

Você agora consegue:

* Extrair texto de qualquer documento
* Lidar com PDFs escaneados
* Salvar e processar resultados
* Preparar dados para LLM

---

Se quiser evoluir:

* embeddings
* busca semântica
* RAG pipeline

Isso já é base sólida pra tudo isso.
