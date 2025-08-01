<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "c4be907703b836d1a1c360db20da4de9",
  "translation_date": "2025-07-12T14:18:47+00:00",
  "source_file": "11-mcp/code_samples/github-mcp/MCP_SETUP.md",
  "language_code": "sr"
}
-->
# MCP Водич за интеграцију сервера

## Захтеви
- Инсталиран Node.js (верзија 14 или новија)
- npm пакет менаџер
- Python окружење са потребним зависностима

## Кораци за подешавање

1. **Инсталирај MCP Server пакет**
   ```bash
   npm install -g @modelcontextprotocol/server-github
   ```

2. **Покрени MCP Server**
   ```bash
   npx @modelcontextprotocol/server-github
   ```
   Сервер би требало да се покрене и прикаже URL за повезивање.

3. **Провери везу**
   - Потражи икону утикача (🔌) у свом Chainlit интерфејсу
   - Поред иконе утикача треба да се појави број (1) који означава успешну везу
   - Конзола треба да прикаже: "GitHub plugin setup completed successfully" (заједно са додатним статусним линијама)

## Решавање проблема

### Чести проблеми

1. **Конфликт порта**
   ```bash
   Error: listen EADDRINUSE: address already in use
   ```
   Решење: Промени порт користећи:
   ```bash
   npx @modelcontextprotocol/server-github --port 3001
   ```

2. **Проблеми са аутентификацијом**
   - Увери се да су GitHub акредитиви исправно подешени
   - Провери да .env фајл садржи потребне токене
   - Потврди приступ GitHub API-ју

3. **Веза није успела**
   - Потврди да сервер ради на очекиваном порту
   - Провери подешавања заштитног зида
   - Увери се да Python окружење има потребне пакете

## Потврда везе

Твој MCP сервер је исправно повезан када:
1. Конзола прикаже "GitHub plugin setup completed successfully"
2. Логови везе покажу "✓ MCP Connection Status: Active"
3. GitHub команде раде у ћаскању

## Променљиве окружења

Потребне у твом .env фајлу:
```
GITHUB_TOKEN=your_github_token
MCP_SERVER_PORT=3000  # Optional, default is 3000
```

## Тестирање везе

Пошаљи ову тест поруку у ћаскању:
```
Show me the repositories for username: [GitHub Username]
```
Успешан одговор ће приказати информације о репозиторијуму.

**Одрицање од одговорности**:  
Овај документ је преведен коришћењем AI услуге за превођење [Co-op Translator](https://github.com/Azure/co-op-translator). Иако се трудимо да превод буде тачан, молимо вас да имате у виду да аутоматски преводи могу садржати грешке или нетачности. Оригинални документ на његовом изворном језику треба сматрати ауторитетним извором. За критичне информације препоручује се професионални људски превод. Нисмо одговорни за било каква неспоразума или погрешна тумачења која произилазе из коришћења овог превода.