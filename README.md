# ZENI AI

Chat responsivo com identidade roxa e preta, uso gratuito sem cadastro, login por e-mail e senha ou Google OAuth, histórico privado por conta e integração com OpenRouter. O backend usa somente a biblioteca padrão do Python; não requer Node.js nem pacotes adicionais.

## Iniciar no PowerShell

Abra um terminal na pasta do projeto e configure a chave do OpenRouter:

```powershell
Set-Location "C:\Users\PEDRO\Downloads\IA-ZENI.A"
$env:OPENROUTER_API_KEY = "sua-chave-do-openrouter"
python server.py
```

Abra <http://127.0.0.1:8000> para conversar sem criar uma conta. A tela de acesso fica em <http://127.0.0.1:8000/login>. Para salvar o histórico na conta, crie uma conta ou entre com e-mail e senha ou Google OAuth. Para encerrar o servidor, use `Ctrl+C` no terminal em que ele está rodando.

Cada conversa aceita até 100 mensagens, contando mensagens do usuário e respostas da ZENI. Ao completar esse total, um novo chat é aberto automaticamente; a conversa anterior continua no histórico. Conversas de visitantes ficam na memória do navegador e não são salvas no histórico da conta. As contas podem ser criadas com nome, e-mail e senha (mínimo de 12 caracteres) ou acessadas pelo Google OAuth quando configurado. As senhas são armazenadas como hashes PBKDF2-SHA256 com salt individual; o navegador nunca recebe o hash. O cookie de sessão é HttpOnly e SameSite=Lax. Para hospedar em HTTPS, configure `$env:ZENI_COOKIE_SECURE = "true"` antes de iniciar. Não há recuperação automática de senha por e-mail.

## Contas existentes e Google OAuth

Contas Google e seus históricos continuam preservados. O acesso pelo Google continua disponível quando as credenciais OAuth estão configuradas. Para adicionar acesso por senha a uma conta Google existente, entre com essa conta, abra **Configurações** e defina uma senha; o histórico e o plano permanecem na mesma conta.

Estas variáveis são opcionais para o chat e para o login por senha; configure-as para habilitar o login Google:

```powershell
$env:GOOGLE_CLIENT_ID = "seu-client-id.apps.googleusercontent.com"
$env:GOOGLE_CLIENT_SECRET = "seu-client-secret"
$env:GOOGLE_REDIRECT_URI = "http://127.0.0.1:8000/auth/google/callback"
```

Para contas novas, Google OAuth não é necessário. Se as variáveis não estiverem configuradas, o cadastro e login por e-mail e senha continuam funcionando normalmente.

## Planos

- **ZENI Free:** até 100 mensagens por conversa, contando as mensagens do usuário e as respostas da ZENI.
- **ZENI Plus:** R$ 15,99 por mês.
- **ZENI Pro:** R$ 29,99 por mês.
- Ao completar 100 mensagens, a ZENI inicia automaticamente uma nova conversa e mantém a anterior no histórico.

Os planos Plus e Pro permanecem listados apenas para consulta e não podem ser assinados no momento.

O SQLite é criado em `zeni.sqlite3` na pasta do projeto. Para mudar o local, configure `ZENI_DATABASE` antes de iniciar o servidor. Conversas e mensagens autenticadas permanecem associadas à conta; conversas de visitantes permanecem apenas na memória do navegador.

## OpenRouter

O servidor usa a API compatível com OpenAI do OpenRouter em `https://openrouter.ai/api/v1`, sem enviar a chave ao navegador.

| Variável | Padrão | Descrição |
| --- | --- | --- |
| `OPENROUTER_API_KEY` | — | Chave privada da conta OpenRouter. |
| `ZENI_MODEL` | `openai/gpt-4o-mini` | Modelo de chat disponível no OpenRouter. |
| `ZENI_HOST` | `127.0.0.1` | Endereço de escuta do servidor. |
| `ZENI_PORT` | `8000` | Porta HTTP local. |
| `GOOGLE_CLIENT_ID` | — | ID OAuth para login com Google. |
| `GOOGLE_CLIENT_SECRET` | — | Segredo OAuth para login com Google. |
| `GOOGLE_REDIRECT_URI` | `http://127.0.0.1:8000/auth/google/callback` | URI de retorno do login Google. |
| `ZENI_COOKIE_SECURE` | `false` | Use `true` quando o site estiver servido exclusivamente por HTTPS. |
| `ZENI_DATABASE` | `zeni.sqlite3` | Caminho opcional do banco SQLite. |

O endpoint `/api/version` mostra a versão do processo servidor; `/api/status` informa se a chave OpenRouter e OAuth estão configurados, sem revelar segredos. A cada chamada, o backend inclui a data e hora atuais de Brasília no contexto da IA.

## Recursos

- Cadastro, login e saída por e-mail e senha, com credenciais validadas no backend.
- Chat gratuito sem cadastro, com até 100 mensagens por conversa e sem persistência de conversas de visitante.
- Login Google opcional e migração de contas existentes para senha, preservando histórico e plano.
- Histórico persistido por usuário, com exclusão de conversas e isolamento entre contas.
- Abertura automática de nova conversa ao completar 100 mensagens; conversas autenticadas anteriores permanecem no histórico.
- Página informativa dos planos Gratuito, Plus e Pro; assinaturas pagas estão indisponíveis.
- Contexto conversacional salvo no servidor, renderização Markdown, cópia de respostas, indicador de processamento e interface móvel.
- Anexos de texto e código até 200 KB.
